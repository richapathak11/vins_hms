<template>
    <div class="modal fade bg-modal-color refdel" id="edit_confirm_modal" role="dialog" aria-labelledby="myModalLabel">
      <div class="modal-dialog confirm-modal" role="document">
        <div class="modal-content">
            <form method="confirm" class="js-confirm-modal-form">
                <div class="modal-header">
                    <h5 class="modal-title" id="myModalLabel">Edit OPD Form</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
                </div>
                <div class="modal-body js-delete-confirmation-msg">{{editConfirmMsg}}</div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-danger"  @click="hideEdit()">Cancel</button>
                    <button type="submit" class="btn btn-primary" @click.prevent="confirmEdit()">Confirm</button>
                </div>
                <input name="_method" value="DELETE" type="hidden" />
            </form>
        </div>
      </div>
    </div>
</template>
<script>
    export default  {
        data() {
            return {

                
            }
        },
        mounted(){
            var vm = this;
            $('.close').click(function(){
                $('#edit_confirm_modal').modal('hide');
                return false;
            });
        },
        props: {
            editConfirmMsg: String
        },
        created() {
            
        },
        methods: {
            confirmEdit() {
                let vm=this;
                vm.$root.$emit('edit_confirmed',1);
            },
            hideEdit() {
                let vm=this;
                $('#edit_confirm_modal').modal('hide');
                return false;
            }
        }
    }
</script>
