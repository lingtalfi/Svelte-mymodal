Svelte mymodal
==========
2020-05-05


A modal component for svelte.



Install
------
```bash
npm i svelte-mymodal
```




How to use
-------

// any.svelte

```svelte
<script>
	import MyModal from "svelte-mymodal";
</script>


<MyModal> 
	<div>I'm a modal</div>
</MyModal>

<MyModal> 
	<div style="border: 2px solid orange;width: 300px;">I'm a modal</div>
</MyModal>

<MyModal open={true} options={{dragHandle: ".header"}}> 
	<div style="border: 2px solid orange;width: 300px;">
		<div class="header" style="background: #ddd; cursor: move;">I'm the header</div>
		<div class="body">I'm the body</div>
	</div>
</MyModal>
```




Doc
----------



props:

- open: bool, controls the modal visibility
- options: 
	- dragHandle: string=null,
         A query selector identifying the drag handle.
         If null, the whole modal serves as a drag handle.
         




my playground
-------

// Test.svelte

```html
<script>

    import MyModal from './my-components/modal/MyModal.svelte';

    let debug = {
        x: 0,
        y: 0,
        dx: 0,
        dy: 0,
    };

    let isOpen = false;


    function handleDragMove(event) {
        debug = event.detail;
    }

    function openModal() {
        isOpen = !isOpen;
    }


    let modalOptions = {
        dragHandle: ".modal-header",
    };
</script>


<div class="debug">
    <span>x: {debug.x}</span><br>
    <span>y: {debug.y}</span><br>
    <span>dx: {debug.dx}</span><br>
    <span>dy: {debug.dy}</span><br>
</div>

<h1 on:click={openModal}>click</h1>
<h1>Here</h1>
<h1>Here</h1>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Cum nisi soluta tempore. Aliquid cum, cupiditate eius enim
    in maiores molestias nam natus nulla quidem quis reprehenderit similique ut, veritatis voluptates!</p>
<h1>Here</h1>
<h1>Open is

    {#if true === isOpen}
        open
    {:else}
        close
    {/if}
</h1>



<MyModal
        on:dragmove={handleDragMove}
        options={modalOptions}
        open={isOpen}
>
    <div class="modal-container">
        <div class="modal-header">
            <h4>Modal header</h4>
        </div>
        <div class="modal-body">
            The modal body is me...
        </div>
        <div class="modal-footer">
            I'm a modal footer
        </div>
    </div>
</MyModal>


<style>


    .modal-container {
        width: 300px;
        height: 200px;
        border-radius: 4px;
        border: 2px solid gray;
        cursor: move;
        padding: 5px;
    }

    .modal-header {
        background: #999999;
        color: white;
        text-transform: uppercase;
    }


</style>



```



// main.js

```js

import Component from './Test.svelte';


window.Test = function () {
    return new Component({
        target: document.body
    });
};

```


History Log
===============

- 1.0.0 -- 2020-05-05

    - initial commit
    




