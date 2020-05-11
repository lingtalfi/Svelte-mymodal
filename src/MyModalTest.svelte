<script>
    import MyModal from "./MyModal.svelte";



</script>


<MyModal {...$$props}>
    <div class="container">
        I'm a modal.
    </div>
</MyModal>



<style>
    .container{
        border: 1px solid black;
        width: 300px;
    }
</style>