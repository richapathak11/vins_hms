<template>
	<div class="col-lg-12 mb-3">
    <div v-if="search_enabled=='true'">
      <patientSearch :searchType="searchType" :page_url="search_page_url" ref="adminPatientSearch"></patientSearch>
    </div>
		<div class="card bg-success-card">
			<h4 class="card-header">
                <div>Waiting patient list </div>
                <div class="text-right"><button type="button" class="btn btn-primary" @click="setSearchType('waiting')">Search</button></div>
            </h4>
            <div class="card-body">
            	<div data-v-744e717e="" class="card p-3"  v-if="(waitingPagination.total > 0)">
              		<div data-v-744e717e="" class="table-header">
                  		<h4 data-v-744e717e="" class="table-title text-center mt-3"></h4>
              		</div>
              		<div data-v-744e717e="" class="table-responsive">
              			<table data-v-744e717e="" class="table">
                  			<thead data-v-744e717e="">
                    			<tr data-v-744e717e="">
                    				 <th data-v-744e717e="" class="sortable sorting-asc " style="width: 200px;">
                                		First Name 
                        			 </th>
                        			 <th style="width: auto;">
                            			Last Name
                             			<i data-v-744e717e="" class="fa float-right"></i> 
                             		</th>
                             		<th data-v-744e717e="" class="sortable" style="width: auto;">
                            			UHID.No
                            			<i data-v-744e717e="" class="fa float-right"></i>
                        			</th>                        			
                        			<th data-v-744e717e="" class="sortable" style="width: auto;">
                            			Age
                            			<i data-v-744e717e="" class="fa float-right"></i>
                        			</th>
                                     <th data-v-744e717e="" class="sortable" style="width: auto;">
                            			Gender
                                       <i data-v-744e717e="" class="fa float-right"></i>
                        			 </th>
                         			 
                        			 <th data-v-744e717e="" class="sortable" style="width: auto;">
                           				 Action
                            			<i data-v-744e717e="" class="fa float-right"></i>
                        			 </th>
                        		</tr>
                  			</thead>
                  			<tbody data-v-744e717e="">
                  				 <tr data-v-744e717e="" v-for="patientData in getWaitingPatientData">
                  				 	<td data-v-744e717e="" class="text-uppercase">
                       					{{ patientData.fname}}
                      				</td>
                      				<td data-v-744e717e="" class="text-uppercase">
                       					{{ patientData.lname}}
                      				</td>
                      				<td data-v-744e717e="" class="text-uppercase">
                        				{{ patientData.uhid_no}}
                      				</td>
                      				<td data-v-744e717e="" class="text-uppercase">

                        		 	<span class="text-uppercase" v-if="(patientData.age > 1000)">
									<span class="text-uppercase" v-if="(((currentYear) - (patientData.age)) > 0)">{{(currentYear) - (patientData.age) - 1}}</span>	
									<span class="text-uppercase" v-if="(((currentYear) - (patientData.age)) == 0)"> 1</span>
									</span>
									<span v-if="(patientData.age  < 1000)">{{patientData.age}}</span>
                      				</td>
                      				<td data-v-744e717e="" class="numeric text-uppercase">
                        				<span v-if="(patientData.gender == 'F')">Female</span>
                        				<span v-if="(patientData.gender == 'M')">Male</span>
                      				</td> 
                      				
                      				<td data-v-744e717e="" class="">
                      					<a :href="'/patient_detail_edit'"> <i class="fa fa-user-md text-info mr-3 text-info mr-3" @click="setPatientId(patientData.patient_id)" title="patient detail form"></i></a>
                                <i class="fa fa-table text-info mr-3 text-info mr-3"  @click="getPatientOPDInfo(patientData.patient_id)" ></i>
                      				</td>
                  				 </tr>
                  			</tbody>
                		</table>
              		</div>
              		<div data-v-744e717e="" class="table-footer">
                		<div data-v-744e717e="" class="datatable-length float-left pl-3">
                  			<span data-v-744e717e="">Rows per page:</span>
                    			<select data-v-744e717e="" class="custom-select" id="perPageNoWaiting"  @change="setPerPageWaiting" v-model="perPageWaiting">
                    				<option data-v-744e717e="" value="2">2</option>
                     				<option data-v-744e717e="" value="5">5</option>
                      				<option data-v-744e717e="" value="10">10</option>
                      				<option data-v-744e717e="" value="20">20</option>
                      				<option data-v-744e717e="" value="50">50</option>
                   				
                    			</select>
                          <div data-v-744e717e="" class="datatable-info  pb-2 mt-6">
                            <ul class="pagination">
                               <li> <a href="javascript:void(0)"  @click="getPatientList('/patient/getallpatientlist?page=1','waiting')" class="previous" v-if="waitingPagination.current_page!=1">&laquo; First</a></li>
                              <li> <a href="javascript:void(0)"  @click="getPatientList(waitingPagination.prev_page_url,'waiting')" class="previous" v-if="waitingPagination.current_page!=1">&laquo; Prev</a></li>
                            <!--   <li  v-for="record_pagination,index in waitingPagination.last_page" v-if="index<=2"><a v-bind:class="[waitingPagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getPatientList('/patient/getallpatientlist?page='+index,'waiting')">{{index}}</a></li> -->
                              <li v-for="record_pagination,index in waitingPagination.last_page" >
                                <span v-if="Math.abs(record_pagination - waitingPagination.current_page)<3">
                                    <a v-bind:class="[waitingPagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getPatientList('/patient/getallpatientlist?page='+index,'waiting')">{{index}}</a>
                                </span>
                              </li> 
                              <li><a href="javascript:void(0)"  v-if="waitingPagination.current_page!=waitingPagination.last_page" @click="getPatientList(waitingPagination.next_page_url,'waiting')" class="next">Next &raquo;</a></li>
                               <li><a href="javascript:void(0)"  v-if="waitingPagination.current_page!=waitingPagination.last_page" @click="getPatientList('/patient/getallpatientlist?page='+waitingPagination.last_page,'waiting')" class="next">Last &raquo;</a></li>
                            </ul>
                          </div>
                     		<div data-v-744e717e="" class="datatable-info  pb-2 mt-3" v-show="(waitingPagination.total > 0)">
                        		<span data-v-744e717e="">Showing </span> {{waitingPagination.from}} - {{waitingPagination.to}} of {{waitingPagination.total}}
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
    <div class="card bg-success-card">
      <h4 class="card-header">
                <div>Examined patient list </div>
                <div class="text-right"><button type="button" class="btn btn-primary" @click="setSearchType('examine')">Search</button></div>
            </h4>
            <div class="card-body">
              <div data-v-744e717e="" class="card p-3" v-if="(examinePagination.total > 0)">
                  <div data-v-744e717e="" class="table-header">
                      <h4 data-v-744e717e="" class="table-title text-center mt-3"></h4>
                  </div>
                  <div data-v-744e717e="" class="table-responsive">
                    <table data-v-744e717e="" class="table">
                        <thead data-v-744e717e="">
                          <tr data-v-744e717e="">
                             <th data-v-744e717e="" class="sortable sorting-asc " style="width: 200px;">
                                    First Name 
                               </th>
                               <th style="width: auto;">
                                  Last Name
                                  <i data-v-744e717e="" class="fa float-right"></i> 
                                </th>
                                <th data-v-744e717e="" class="sortable" style="width: auto;">
                                  UHID.No
                                  <i data-v-744e717e="" class="fa float-right"></i>
                              </th>                             
                              <th data-v-744e717e="" class="sortable" style="width: auto;">
                                  Age
                                  <i data-v-744e717e="" class="fa float-right"></i>
                              </th>
                                     <th data-v-744e717e="" class="sortable" style="width: auto;">
                                  Gender
                                       <i data-v-744e717e="" class="fa float-right"></i>
                               </th>
                               
                               <th data-v-744e717e="" class="sortable" style="width: auto;">
                                   Action
                                  <i data-v-744e717e="" class="fa float-right"></i>
                               </th>
                            </tr>
                        </thead>
                        <tbody data-v-744e717e="">
                           <tr data-v-744e717e="" v-for="patientData in getExaminePatientData">
                            <td data-v-744e717e="" class="text-uppercase">
                                {{ patientData.fname}}
                              </td>
                              <td data-v-744e717e="" class="text-uppercase">
                                {{ patientData.lname}}
                              </td>
                              <td data-v-744e717e="" class="text-uppercase">
                                {{ patientData.uhid_no}}
                              </td>
                              <td data-v-744e717e="" class="text-uppercase">

                              <span class="text-uppercase" v-if="(patientData.age > 1000)">
                  <span class="text-uppercase" v-if="(((currentYear) - (patientData.age)) > 0)">{{(currentYear) - (patientData.age) - 1}}</span>  
                  <span class="text-uppercase" v-if="(((currentYear) - (patientData.age)) == 0)"> 1</span>
                  </span>
                  <span v-if="(patientData.age  < 1000)">{{patientData.age}}</span>
                              </td>
                              <td data-v-744e717e="" class="numeric text-uppercase">
                                <span v-if="(patientData.gender == 'F')">Female</span>
                                <span v-if="(patientData.gender == 'M')">Male</span>
                              </td> 
                              
                              <td data-v-744e717e="" class="">
                                <a :href="'/patient_detail_edit'"> <i class="fa fa-user-md text-info mr-3 text-info mr-3" @click="setPatientId(patientData.patient_id)" title="patient detail form"></i></a>
                                <i class="fa fa-table text-info mr-3 text-info mr-3"  @click="getPatientOPDInfo(patientData.patient_id)" ></i>
                              </td>
                           </tr>
                        </tbody>
                    </table>
                  </div>
                  <div data-v-744e717e="" class="table-footer">
                    <div data-v-744e717e="" class="datatable-length float-left pl-3">
                        <span data-v-744e717e="">Rows per page:</span>
                          <select data-v-744e717e="" class="custom-select" id="perPageNoExamine"  @change="setPerPageExamine" v-model="perPageExamine">
                            <option data-v-744e717e="" value="2">2</option>
                            <option data-v-744e717e="" value="5">5</option>
                              <option data-v-744e717e="" value="10">10</option>
                              <option data-v-744e717e="" value="20">20</option>
                              <option data-v-744e717e="" value="50">50</option>
                            <!--     <option data-v-744e717e="" value="-1">All</option> -->
                          </select>
                          <div data-v-744e717e="" class="datatable-info  pb-2 mt-6">
                            <ul class="pagination">
                               <li> <a href="javascript:void(0)"  @click="getPatientList('/patient/getallpatientlist?page=1','examine')" class="previous" v-if="examinePagination.current_page!=1">&laquo; First</a></li>
                              <li> <a href="javascript:void(0)"  @click="getPatientList(examinePagination.prev_page_url,'examine')" class="previous" v-if="examinePagination.current_page!=1">&laquo; Prev</a></li>
                              <!-- <li  v-for="record_pagination,index in examinePagination.last_page" v-if="index<=2"><a v-bind:class="[examinePagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getPatientList('/patient/getallpatientlist?page='+index,'examine')">{{index}}</a></li> -->
                               <li v-for="record_pagination,index in examinePagination.last_page" >
                                <span v-if="Math.abs(record_pagination - examinePagination.current_page)<3">
                                    <a v-bind:class="[examinePagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getPatientList('/patient/getallpatientlist?page='+index,'examine')">{{index}}</a>
                                </span>
                              </li> 
                              <li><a href="javascript:void(0)"  v-if="examinePagination.current_page!=examinePagination.last_page" @click="getPatientList(examinePagination.next_page_url,'examine')" class="next">Next &raquo;</a></li>
                              <li><a href="javascript:void(0)"  v-if="examinePagination.current_page!=examinePagination.last_page" @click="getPatientList('/patient/getallpatientlist?page='+examinePagination.last_page,'examine')" class="next">Last &raquo;</a></li>
                            </ul>
                          </div>
                        <div data-v-744e717e="" class="datatable-info  pb-2 mt-3" v-show="(examinePagination.total > 0)">
                            <span data-v-744e717e="">Showing </span> {{examinePagination.from}} - {{examinePagination.to}} of {{examinePagination.total}}
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
    <div class="card bg-success-card">
      <h4 class="card-header">
          <div>Report patient list </div>
          <div class="text-right"><button type="button" class="btn btn-primary" @click="setSearchType('reports')">Search</button></div>
      </h4>
      <div class="card-body">
         <div data-v-744e717e="" class="card p-3" v-if="(reportPagination.total > 0)">
           <div data-v-744e717e="" class="table-header">
              <h4 data-v-744e717e="" class="table-title text-center mt-3"></h4>
            </div>
            <div data-v-744e717e="" class="table-responsive">
                    <table data-v-744e717e="" class="table" >
                        <thead data-v-744e717e="">
                          <tr data-v-744e717e="">
                             <th data-v-744e717e="" class="sortable sorting-asc " style="width: 200px;">
                                    First Name 
                               </th>
                               <th style="width: auto;">
                                  Last Name
                                  <i data-v-744e717e="" class="fa float-right"></i> 
                                </th>
                                <th data-v-744e717e="" class="sortable" style="width: auto;">
                                  UHID.No
                                  <i data-v-744e717e="" class="fa float-right"></i>
                              </th>                             
                              <th data-v-744e717e="" class="sortable" style="width: auto;">
                                  Age
                                  <i data-v-744e717e="" class="fa float-right"></i>
                              </th>
                                     <th data-v-744e717e="" class="sortable" style="width: auto;">
                                  Gender
                                       <i data-v-744e717e="" class="fa float-right"></i>
                               </th>
                               
                               <th data-v-744e717e="" class="sortable" style="width: auto;">
                                   Action
                                  <i data-v-744e717e="" class="fa float-right"></i>
                               </th>
                            </tr>
                        </thead>
                        <tbody data-v-744e717e="">
                           <tr data-v-744e717e="" v-for="patientData in getReportPatientData">
                            <td data-v-744e717e="" class="text-uppercase">
                                {{ patientData.fname}}
                              </td>
                              <td data-v-744e717e="" class="text-uppercase">
                                {{ patientData.lname}}
                              </td>
                              <td data-v-744e717e="" class="text-uppercase">
                                {{ patientData.uhid_no}}
                              </td>
                              <td data-v-744e717e="" class="text-uppercase">

                              <span class="text-uppercase" v-if="(patientData.age > 1000)">
                  <span class="text-uppercase" v-if="(((currentYear) - (patientData.age)) > 0)">{{(currentYear) - (patientData.age) - 1}}</span>  
                  <span class="text-uppercase" v-if="(((currentYear) - (patientData.age)) == 0)"> 1</span>
                  </span>
                  <span v-if="(patientData.age  < 1000)">{{patientData.age}}</span>
                              </td>
                              <td data-v-744e717e="" class="numeric text-uppercase">
                                <span v-if="(patientData.gender == 'F')">Female</span>
                                <span v-if="(patientData.gender == 'M')">Male</span>
                              </td> 
                              
                              <td data-v-744e717e="" class="">
                                <a :href="'/patient_detail_edit'"> <i class="fa fa-user-md text-info mr-3 text-info mr-3" @click="setPatientId(patientData.patient_id)" title="patient detail form"></i></a>
                                <i class="fa fa-table text-info mr-3 text-info mr-3"  @click="getPatientOPDInfo(patientData.patient_id)" ></i>
                              </td>
                           </tr>
                        </tbody>
                    </table>
                  </div>
                  <div data-v-744e717e="" class="table-footer">
                    <div data-v-744e717e="" class="datatable-length float-left pl-3">
                        <span data-v-744e717e="">Rows per page:</span>
                          <select data-v-744e717e="" class="custom-select" id="perPageNoReport"  @change="setPerPageReport" v-model="perPageReport">
                            <option data-v-744e717e="" value="2">2</option>
                            <option data-v-744e717e="" value="5">5</option>
                              <option data-v-744e717e="" value="10">10</option>
                              <option data-v-744e717e="" value="20">20</option>
                              <option data-v-744e717e="" value="50">50</option>
                            <!--     <option data-v-744e717e="" value="-1">All</option> -->
                          </select>
                          <div data-v-744e717e="" class="datatable-info  pb-2 mt-6">
                            <ul class="pagination">
                              <li> <a href="javascript:void(0)"  @click="getPatientList('/patient/getallpatientlist?page=1','reports')" class="previous" v-if="reportPagination.current_page!=1">&laquo; First</a></li>
                              <li> <a href="javascript:void(0)"  @click="getPatientList(reportPagination.prev_page_url,'reports')" class="previous" v-if="reportPagination.current_page!=1">&laquo; Prev</a></li>
                            <!--   <li  v-for="record_pagination,index in reportPagination.last_page" v-if="index<=2"><a v-bind:class="[reportPagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getPatientList('/patient/getallpatientlist?page='+index,'reports')">{{index}}</a></li> -->
                             <li v-for="record_pagination,index in reportPagination.last_page" >
                                <span v-if="Math.abs(record_pagination - reportPagination.current_page)<3">
                                    <a v-bind:class="[reportPagination.current_page==++index ? 'active' : '']" href="javascript:void(0)" @click="getPatientList('/patient/getallpatientlist?page='+index,'reports')">{{index}}</a>
                                </span>
                              </li> 
                              <li><a href="javascript:void(0)"  v-if="reportPagination.current_page!=reportPagination.last_page" @click="getPatientList(reportPagination.next_page_url,'reports')" class="next">Next &raquo;</a></li>
                               <li><a href="javascript:void(0)"  v-if="reportPagination.current_page!=reportPagination.last_page" @click="getPatientList('/patient/getallpatientlist?page='+reportPagination.last_page,'reports')" class="next">Last &raquo;</a></li>
                            </ul>
                          </div>
                        <div data-v-744e717e="" class="datatable-info  pb-2 mt-3" v-show="(reportPagination.total > 0)">
                            <span data-v-744e717e="">Showing </span> {{reportPagination.from}} - {{reportPagination.to}} of {{reportPagination.total}}
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
     <span v-if="open_opd_modal"> 
          <patientOPDInfoTable ref="modal" :patientDataID="pid"></patientOPDInfoTable>
        </span>
	</div>
</template>
<script>
	import User from '../../../api/users.js';
  import patientOPDInfoTable from './patientOPDInfoTable.vue';
  import patientSearch from './patientSearchDataAdmin.vue';
	export default {
		 data() {
		 	return {
        'login_user_id' :this.$store.state.Users.userDetails.id,
		 	  'currentYear': (new Date()).getFullYear(),
		 	  'user':this.$store.state.Users.userDetails.first_name + " "+ this.$store.state.Users.userDetails.last_name ,
              'user_id':0,
              'user_type':this.$store.state.Users.userDetails.user_type,
              'getPatientData' : '',
              'getWaitingPatientData' : '',
              'getExaminePatientData' : '',
              'getReportPatientData' : '',
              'pagination': {},
              'perPage' : 5,
              'perPageWaiting' : 5,
              'perPageExamine' : 5,
              'perPageReport' : 5,
              'patientId' :'',
              'open_opd_modal':false,
              'getPatientOPDInfo ' : '',
              'waitingPagination': {},
              'examinePagination': {},
              'reportPagination': {},
              'searchType':'',
              'search_enabled':false,
              'search_no_page':5,
              'search_data':{},
              'search_page_url':'/patient/getPatientListSearchAdmin',
		 	}
		 },
       created: function() {
             this.$root.$on('close_modal', this.close_modal);
             this.$root.$on('searchPatientData', this.searchPatientData);
             this.$root.$on('patientEmpty', this.setSearchData);
        },
		  mounted(){
		 	let vm = this;
      vm.getUserRole('list.patient');
		 	 // if(vm.$store.state.Users.userDetails.user_type != '4'){
     //          vm.$root.$emit('logout','You are not authorise to access this page'); 
     //      	}
		 //	let page_url = '/patient/getallpatientlist';
       vm.getPatientList('/patient/getallpatientlist','waiting');
       vm.getPatientList('/patient/getallpatientlist','examine');
       vm.getPatientList('/patient/getallpatientlist','reports');
		 //	vm.getPatientList(page_url);
		 },
     components: {
        patientOPDInfoTable,
        patientSearch
    },
		 methods:{
       getUserRole(permission){
                 var vm = this;
                User.getUserRole(vm.login_user_id,permission).then(
                    (responce) => {
                       if(responce.data.data == ''){
                         vm.$root.$emit('logout','You are not authorise to access this page');
                       }
                    },
                    (error) =>{

                    }
                    );
            },
      setSearchType(stype)
      {
          let vm=this;
          vm.search_enabled='true';
          vm.searchType=stype;
      },
      searchPatientData(patData,page_url)
      {
          let vm=this;
          vm.search_data=patData;
          let no_of_page = '';
           if(vm.searchType  == 'waiting'){
               no_of_page = vm.perPageWaiting;
            }
             if(vm.searchType  == 'examine'){
               no_of_page = vm.perPageExamine;
            }
            if(vm.searchType  == 'reports'){
               no_of_page = vm.perPageReport;
            }

           User.generatePatientListSearchByAdminPagintion(page_url,patData,no_of_page).then(
                (response) => {
                    if(response.data.code == 200) {
                        var pData =response;
                        if(vm.searchType == 'waiting'){
                          vm.getWaitingPatientData =pData.data.data.data;
                        }
                        else if(vm.searchType == 'examine'){
                          vm.getExaminePatientData =pData.data.data.data;
                        }
                        else if(vm.searchType == 'report'){
                          vm.getReportPatientData =pData.data.data.data;
                        }
                        vm.makePagination(pData.data.data,vm.searchType);
                    } else if(response.data.code == 300) {
                        toastr.error('Record not found', 'Error', {timeOut: 5000});
                        this.$root.$emit('patientEmpty',1);
                    } else{
                        toastr.error('Something goes wrong', 'Error', {timeOut: 5000});
                        this.$root.$emit('patientEmpty',1);
                    }
                       
                    $("body .js-loader").addClass('d-none');
                },
                (error) => {
                     $("body .js-loader").addClass('d-none');

        });
        vm.search_enabled='false';
        //vm.setSearchData();
      },
      setSearchData()
      {
          let vm=this;
          vm.search_enabled='false';
          vm.searchType='';
      },
     close_modal()
        {
            this.pid='';
            this.open_opd_modal=false;
        },
         getPatientOPDInfo(p_id)
        {
            this.pid=p_id;
            this.open_opd_modal=true;
            setTimeout(function(){
              $('#patientOPDModal').modal('show');
            },500)
        },
        close: function () {
            this.$emit('close');
            this.title = '';
            this.body = '';
          },
		 	getPatientList(page_url,status){
		 		let vm = this;
		 		let userId = vm.user_id;
		 		let userType = vm.user_type;
		 		//let status = '';
		 		//let no_of_page = vm.perPage;
        let no_of_page = '';
           if(status == 'waiting'){
               no_of_page = vm.perPageWaiting;
            }
             if(status == 'examine'){
               no_of_page = vm.perPageExamine;
            }
            if(status == 'reports'){
               no_of_page = vm.perPageReport;
            }

		 		User.getAllPatientListWithPaggination(page_url,status,userType,no_of_page,userId).then(
		 			 (response) => {
             if(status == 'waiting'){
               vm.getWaitingPatientData = response.data.data.data;
            }else if(status == 'examine'){
               vm.getExaminePatientData = response.data.data.data;
            }else if(status == 'reports'){
               vm.getReportPatientData = response.data.data.data;
            }else{
              vm.getPatientData = response.data.data.data;
            }
		 			 	vm.makePagination( response.data.data,status);
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
 
                if(status == 'waiting') {

                  this.waitingPagination = pagination;
                }
                 if(status == 'examine') { 

                  this.examinePagination = pagination;
                }
                if(status == 'reports') {

                  this.reportPagination = pagination;
                }
                else {
                  this.pagination = pagination;
                }
                

                //this.$set('pagination', pagination)
            },
          setPatientId(patientInfo){
             var vm =this;
            vm.patientId = patientInfo;
            vm.$store.dispatch('SetPatientId', vm.patientId);             // 
          },
          setPerPage(e){
            let vm =this;
            if(vm.searchType == 'waiting' || vm.searchType == 'examine'|| vm.searchType == 'reports')
            {
                vm.searchPatientData(vm.search_data,'/patient/getPatientListSearchAdmin');
            }
            else
            {
                // vm.getPatientList('/patient/getallpatientlist');
              vm.getPatientList('/patient/getallpatientlist','waiting');
              vm.getPatientList('/patient/getallpatientlist','examine');
              vm.getPatientList('/patient/getallpatientlist','reports');
            }
              
          },
           setPerPageExamine(e){
            let vm =this;

            if(vm.searchType == 'examine')
            {
                vm.searchPatientData(vm.search_data,'/patient/getPatientListSearchAdmin');
            }
            else
            {
                // vm.getPatientList('/patient/getallpatientlist');
              vm.getPatientList('/patient/getallpatientlist','waiting');
              vm.getPatientList('/patient/getallpatientlist','examine');
              vm.getPatientList('/patient/getallpatientlist','reports');
            }
              // vm.getPatientList('/patient/getallpatientlist');
            
          },
           setPerPageWaiting(e){
            let vm =this;
            if(vm.searchType == 'waiting')
            {
                vm.searchPatientData(vm.search_data,'/patient/getPatientListSearchAdmin');
            }
            else
            {
              vm.getPatientList('/patient/getallpatientlist','waiting');
              vm.getPatientList('/patient/getallpatientlist','examine');
              vm.getPatientList('/patient/getallpatientlist','reports');
            }
          },
           setPerPageReport(e){
            let vm =this;
            if(vm.searchType == 'reports')
            {
                vm.searchPatientData(vm.search_data,'/patient/getPatientListSearchAdmin');  
            }
            else
            {
                // vm.getPatientList('/patient/getallpatientlist');
              vm.getPatientList('/patient/getallpatientlist','waiting');
              vm.getPatientList('/patient/getallpatientlist','examine');
              vm.getPatientList('/patient/getallpatientlist','reports');
            }
          }

		 },
		
	}
</script>