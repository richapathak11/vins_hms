<template>
	<div class="col-lg-12 mb-3">
   
		<div class="card bg-success-card">
      <div class="card-header">
        <div class="row">
          <div class="col-md-6"><h4>Special Request List</h4></div>
          <div  class="col-md-6 text-right"><button type="button" class="btn btn-primary" @click="setAddSpecialRequest()">Add</button></div></div>
        </div>
		
            <div class="card-body">
            	<div data-v-744e717e="" class="card p-3"  v-if="(specialRequestPagination.total > 0)">
              		<div data-v-744e717e="" class="table-header">
                  		<h4 data-v-744e717e="" class="table-title text-center mt-3"></h4>
              		</div>
              		<div data-v-744e717e="" class="table-responsive">
              			<table data-v-744e717e="" class="table">
                  			<thead data-v-744e717e="">
                    			<tr data-v-744e717e="">
                    				 <th data-v-744e717e="" class="sortable sorting-asc " style="">
                                		Radiology
                        			 </th>
                        			 <th style="width: auto;">
                            			Name
                             			<i data-v-744e717e="" class="fa float-right"></i> 
                             		</th>
                               
                        			 <th data-v-744e717e="" class="sortable" style="width: auto;">
                           				 Action
                            			<i data-v-744e717e="" class="fa float-right"></i>
                        			 </th>
                        		</tr>
                  			</thead>
                  			<tbody data-v-744e717e=""  v-for="specialRequestData in getSpecialRequestData">
                  				 <tr data-v-744e717e="" :id="'presp_'+specialRequestData.special_id">
                  				 	<td data-v-744e717e="" class="text-uppercase">
                       					{{ specialRequestData.radio_name}}
                      				</td>
                      				<td data-v-744e717e="" class="text-uppercase">
                                {{ specialRequestData.special_name}}
                      				</td>
                      				<td data-v-744e717e="" class="">
                      					<a> <i class="fa fa-remove text-danger mr-3 text-info mr-3" @click="removeSpecialRequest(specialRequestData.special_id)" title="Special Request Delete"></i></a>
                                <a  @click="setSpecialRequestId(specialRequestData.special_id)" title="Special Request Edit"> <i class="fa fa-pencil text-info mr-3 text-info mr-3" ></i></a>
                      				</td>
                  				 </tr>
                  			</tbody>
                		</table>
              		</div>
              		<div data-v-744e717e="" class="table-footer">
                		<div data-v-744e717e="" class="datatable-length float-left pl-3">
                  			<span data-v-744e717e="">Rows per page:</span>
                    			<select data-v-744e717e="" class="custom-select" id="perPageNoSpecialRequest"  @change="setPerPageSpecialRequest" v-model="perPageSpecialRequest">
                    				<option data-v-744e717e="" value="2">2</option>
                     				<option data-v-744e717e="" value="5">5</option>
                      				<option data-v-744e717e="" value="10">10</option>
                      				<option data-v-744e717e="" value="20">20</option>
                      				<option data-v-744e717e="" value="50">50</option>
                   					<!--     <option data-v-744e717e="" value="-1">All</option> -->
                    			</select>
                           <div data-v-744e717e="" class="datatable-info  pb-2 mt-6">
                            <ul class="pagination">
                              <li> <a href="javascript:void(0)"  @click="getSpecialRequestList('/specialRequest/getSpecialRequestList?page=1',perPage)" class="previous" v-if="specialRequestPagination.current_page!=1">&laquo; First</a></li>
                              <li> <a href="javascript:void(0)"  @click="getSpecialRequestList(specialRequestPagination.prev_page_url)" class="previous" v-if="specialRequestPagination.current_page!=1">&laquo; Prev</a></li>
                             <!--  <li  v-for="record_pagination,index in specialRequestPagination.last_page" v-if="index<=2"><a v-bind:class="[specialRequestPagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getSpecialRequestList('/specialRequest/getSpecialRequestList?page='+index)">{{index}}</a></li> -->
                             <li v-for="record_pagination,index in specialRequestPagination.last_page" >
                              <span v-if="Math.abs(record_pagination - specialRequestPagination.current_page)<3">
                                  <a v-bind:class="[specialRequestPagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getSpecialRequestList('/specialRequest/getSpecialRequestList?page='+index,perPage)">{{index}}</a>
                              </span>
                            </li> 
                              <li><a href="javascript:void(0)"  v-if="specialRequestPagination.current_page!=specialRequestPagination.last_page" @click="getSpecialRequestList(specialRequestPagination.next_page_url)" class="next">Next &raquo;</a></li>
                               <li><a href="javascript:void(0)"  v-if="specialRequestPagination.current_page!=specialRequestPagination.last_page" @click="getSpecialRequestList('/specialRequest/getSpecialRequestList?page='+specialRequestPagination.last_page,perPage)" class="next">Last &raquo;</a></li>
                            </ul>
                          </div>
                     		<div data-v-744e717e="" class="datatable-info  pb-2 mt-3" v-show="(specialRequestPagination.total > 0)">
                        		<span data-v-744e717e="">Showing </span> {{specialRequestPagination.from}} - {{specialRequestPagination.to}} of {{specialRequestPagination.total}}
                        		<span data-v-744e717e="">records</span>
                   			</div>
               			</div>
              		 </div>
              	</div>
                  <div data-v-744e717e="" class="card p-3" v-else>
                     <div data-v-744e717e="" class="table-responsive">
                         <span> No Record Found</span>
                      </div>
                 </div>
            </div>
		</div>
	</div>
</template>
<script>
	import User from '../../../api/users.js';
  
	export default {
		 data() {
		 	return {
		 	  'currentYear': (new Date()).getFullYear(),
		 	  'user':this.$store.state.Users.userDetails.first_name + " "+ this.$store.state.Users.userDetails.last_name ,
        'user_id':0,
        'user_type':this.$store.state.Users.userDetails.user_type,
        'getSpecialRequestData' : '',
        'pagination': {},
        'perPage' : 5,
        'perPageSpecialRequest' : 5,
        'specialRequestPagination': {},
        'import_file':''
		 	}
		 },
       created: function() {
            
        },
		  mounted(){
		 	let vm = this;
		 	 if(vm.$store.state.Users.userDetails.user_type != '4'){
          vm.$root.$emit('logout','You are not authorise to access this page'); 
        }
		
       vm.getSpecialRequestList('/specialRequest/getSpecialRequestList');
      
		 },
     components: {
      
    },
		 methods:{
      verifyImportFile: function(event)
      {
          let files=this.$refs.file.files[0];
          if(files)
          {
              return true;
          }
          else
          {
            toastr.error('Please Add Files.', 'Add Special Request', {timeOut: 5000});
            event.preventDefault();
          }
           
      },
      removeSpecialRequest(id)
      {
          let vm=this;
            User.deleteSpecialRequest(id).then(
                (response)=> {
                 
                  if(response.data.code == 200){
                    //$('#presp_'+id).remove();
                    vm.getSpecialRequestList('/specialRequest/getSpecialRequestList');
                    toastr.success('Special Request deleted successfully', 'Add Special Request', {timeOut: 5000});
                      //this.initialState();
                      
                  } else if (response.data.code == 300) {
                      toastr.error('Something Went wrong.', 'Add Special Request', {timeOut: 5000});
                      //this.initialState(); 
                  }
                  else
                  {
                      toastr.error('Something Went wrong.', 'Add Special Request', {timeOut: 5000});
                  }
                  
                },
                (error)=>{
                }

              )
      },
      setSpecialRequestId(id)
      {
        let vm=this;
          vm.$store.dispatch('SetSpecialRequestId', id); 
          vm.$store.dispatch('SetSpecialRequestPage','EDIT');
          vm.$router.push({'name':'specialRequest_add'});
      },
      setAddSpecialRequest()
      {
          let vm=this;
          vm.$store.dispatch('SetSpecialRequestId', ''); 
          vm.$store.dispatch('SetSpecialRequestPage','ADD');
          vm.$router.push({'name':'specialRequest_add'});
      },
		 	getSpecialRequestList(page_url){
		 		let vm = this;
		 		let userId = vm.user_id;
		 		let userType = vm.user_type;
		 	
        let no_of_page = '';
        no_of_page = vm.perPageSpecialRequest;

		 		User.getSpecialRequestList(page_url,userType,no_of_page,userId).then(
		 			 (response) => {
              vm.getSpecialRequestData = response.data.data.data;
		 			 	  vm.makePagination( response.data.data);
		 			 },
		 			 (error) => {
                  	 },
		 		);
		 	},
		 	makePagination: function(data,status){
          let pagination = {
              current_page: data.current_page,
              last_page: data.last_page,
              next_page_url: data.next_page_url,
              prev_page_url: data.prev_page_url,
              total : data.total,
              from : data.from,
              to : data.to
          }
          this.specialRequestPagination = pagination;
      },
          
    setPerPage(e){
      let vm =this;
      vm.getSpecialRequestList('/specialRequest/getSpecialRequestList');
    },
    setPerPageSpecialRequest(e){
      let vm =this;
      vm.getSpecialRequestList('/specialRequest/getSpecialRequestList');
    },
	},
		
}
</script>