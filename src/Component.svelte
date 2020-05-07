<script>


    import {writable} from 'svelte/store';
    import {createEventDispatcher} from 'svelte';
    import {beforeUpdate} from 'svelte';
    import draggable from "svelte-action-draggable";


    export let options = {};
    export let open = true;


    let maxTop = 0;
    let maxRight = 0;
    let maxBottom = 0;
    let maxLeft = 0;


    const coords = writable({x: 0, y: 0});
    const styleId = "my-draggable-tmp-style";
    const dispatch = createEventDispatcher();
    let draggableOptions = {
        startClick: "main",
        onStartDragBefore: function (event) {

            if (null !== modalOptions.dragHandle) {
                let dragHandle = event.target.closest(modalOptions.dragHandle);
                if (null === dragHandle) {
                    return false;
                }
            }
        },
    };
    let displayStyle = '';


    let modalOptions = Object.assign({
        /**
         * A selector identifying the drag handle.
         * If null, the whole modal serves as a drag handle.
         */
        dragHandle: null,
        /**
         * Whether the modal should be trapped into the viewport.
         * If false, the user can drag the modal out of the window (and loose it).
         */
        keepInViewport: true,
    }, options);


    function handleDragMove(event) {
        coords.update($coords => {
            let newX = $coords.x + event.detail.dx;
            let newY = $coords.y + event.detail.dy;


            if (true === modalOptions.keepInViewport) {
                if (newX < maxLeft) {
                    newX = maxLeft;
                }
                if (newX > maxRight) {
                    newX = maxRight;
                }
                if (newY < maxTop) {
                    newY = maxTop;
                }
                if (newY > maxBottom) {
                    newY = maxBottom;
                }
            }

            return ({
                x: newX,
                y: newY,
            });
        });

        dispatch('dragmove', event.detail);


    }

    function handleDragStart(event) {

        //----------------------------------------
        // REMOVE BACKGROUND SELECTION WHILE DRAGGING
        //----------------------------------------
        let elStyle = document.getElementById(styleId);
        if (null === elStyle) {
            var elHead = document.getElementsByTagName('head')[0];
            elStyle = document.createElement('style');
            elStyle.type = 'text/css';
            elHead.appendChild(elStyle);
            elStyle.id = styleId;
            elStyle.innerHTML = '.draggable-unselectable > * {user-select: none;}';
        }
        this.classList.add('draggable-unselectable');


        if (true === modalOptions.keepInViewport) {

            let r = event.target.firstChild.getBoundingClientRect();
            maxLeft = -r.left + $coords.x;
            maxRight = window.innerWidth - r.right + $coords.x;
            maxTop = -r.top + $coords.y;
            maxBottom = window.innerHeight - r.bottom + $coords.y;
        }

    }

    function handleDragEnd(event) {
        this.classList.remove('draggable-unselectable');
        let elStyle = document.getElementById(styleId);
        if (null !== elStyle) {
            elStyle.remove();
        }
    }


    //----------------------------------------
    // POLYFILLS
    //----------------------------------------
    // CustomEvent polyfill ie11
    // https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/CustomEvent#Polyfill
    (function () {
        if (typeof window.CustomEvent === "function") return false;

        function CustomEvent(event, params) {
            params = params || {bubbles: false, cancelable: false, detail: null};
            var evt = document.createEvent('CustomEvent');
            evt.initCustomEvent(event, params.bubbles, params.cancelable, params.detail);
            return evt;
        }

        window.CustomEvent = CustomEvent;
    })();


    beforeUpdate(() => {
        if (true === open) {
            // note: could be display: block, or display: inline, or etc... so we don't set it
            displayStyle = '';
        } else {
            displayStyle = 'display: none;'
        }
    });

    function closeModal() {
        open = false;
    }


</script>


<div
        use:draggable={draggableOptions}
        on:dragstart={handleDragStart}
        on:dragmove={handleDragMove}
        on:dragend={handleDragEnd}
        style="
        {displayStyle}
        transform: translate({$coords.x}px,{$coords.y}px)
        "
>
    <slot></slot>
</div>


