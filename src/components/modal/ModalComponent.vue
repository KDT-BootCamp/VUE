<template>
    <div class="container">
        <div class="modal-wraaper">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title">
                            <slot name="title"></slot>
                        </h5>
                        <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <slot name="body"></slot>
                    </div>
                    <div class="modal-footer">
                        <slot name="footer"></slot>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>



export default {

 //   emits : ['close', 'delete'] ,

    setup( ) {
      /*  const onClose = () => {
            emit('close');
        }
        const onDelete = () => {
            emit('delete');
        }
    */
    
    return {
        //onClose,
        //onDelete,

    }
    },
}
</script>

<style>
    .modal-wraaper {
        position : fixed ;
        top : 0;
        left : 0;
        width : 100%;
        height : 100%;
        background-color: rgba(0,0,0,0.5); 
    }
</style>