<template>
  <div class="container">
  	<div class="page-header">
  		<div class="row">
  			<div class="col-md-6">
  			<h1>Culture / Biopsy Form</h1>
  			</div>
  		</div>
  	</div>

  	<form name="" method="post">


      <div class="row form-group">
        <div class="col-md-6">
          <div class="col-md-6">
            <label>IPD No. :</label>
          </div>
          <div class="col-md-6">
            <input class="form-control" name="ipd_id" type="text" id="ipd_id" v-model="ipd_id" v-validate="'required|numeric'" />
            <span class="help is-danger" v-show="errors.has('ipd_id')">
              Numeric Field is required
            </span>
          </div>
        </div>
        <div class="col-md-6">
          <div class="text-right">
            <addressograph></addressograph>
          </div>
       </div>
      </div>

      <div class="row form-group">
        <div class="col-md-6">
          <div class="col-md-6">
            <label>Date:</label>
          </div>
          <div class="col-md-6">
            <input type="text" class="form-control ls-datepicker" id = "date_culture" name="text" v-validate="'required'" v-model="cultureBiopsyData.date_culture">
            <span class="help is-danger" v-show="errors.has('date_culture')">
              Field is required
            </span>
          </div>
        </div>
      </div>
      <div class="row form-group">
        <div class="col-md-6">
          <div class="col-md-6">
            <label>Time :</label>
          </div>
          <div class="col-md-6">

            <input type="text" class="form-control ls-timepicker" name="time_culture" v-validate="'required'" v-model="cultureBiopsyData.time_culture" id ="time_culture">
            <span class="help is-danger" v-show="errors.has('time_culture')">
              Field is required
            </span>
          </div>
        </div>
        <div class="col-md-6">
          <div class="col-md-6">
            <label>Doctor's Name : </label>
          </div>
          <div class="col-md-6">
            <input type="text" class="form-control" name="doctors_name" v-validate="'required'" v-model="cultureBiopsyData.doctors_name" id="doctors_name">
            <span class="help is-danger" v-show="errors.has('doctors_name')">
              Field is required
            </span>
          </div>
        </div>
      </div>

      <div class="row form-group">
        <div class="col-md-6">
          <div class="col-md-6">
            <label>Given By : </label>
          </div>
          <div class="col-md-6">
            <input type="text" class="form-control" name="given_by" v-validate="'required'" v-model="cultureBiopsyData.given_by" id="given_by">
            <span class="help is-danger" v-show="errors.has('given_by')">
              Field is required
            </span>
          </div>
        </div>
        <div class="col-md-6">
          <div class="col-md-6">
            <label>Recieved By : </label>
          </div>
          <div class="col-md-6">
            <input type="text" class="form-control" name="received_by" v-validate="'required'" v-model="cultureBiopsyData.received_by" id="received_by">
            <span class="help is-danger" v-show="errors.has('consulting_dr')">
              Field is required
            </span>
          </div>
        </div>
      </div>


  		<hr>

  		<table class="table table-bordered">
  		    <thead>
  				  <tr>
    					<th colspan="2" class="col-md-6 text-center">Histopathology</th>
    					<th colspan="2" class="col-md-6 text-center">Cytology</th>
  				  </tr>
  				</thead>
  				<tbody>
            <tr>
              <td class="col-md-3"><label class="control-label" for="ihc">IHC:</label></td>
              <td class="col-md-3"><input type="text" class="form-control" name="ihc" v-validate="'required'" v-model="cultureBiopsyData.ihc" id="ihc">
                <span class="help is-danger" v-show="errors.has('ihc')">
                  Field is required
                </span>
              </td>
              <td colspan="2">
                <select class="form-control ls-select2" name="cytology" v-validate="'required'" v-model="cultureBiopsyData.cytology"  value="">
                  <option value="culture">Culture</option>
                  <option value="sensitivity">Sensitivity</option>
                </select>
                <span class="help is-danger" v-show="errors.has('cytology')">
                  Field is required
                </span>
              </td>
            </tr>
            <tr>
              <td class="col-md-3"><label class="control-label" for="others">Others:</label></td>
              <td class="col-md-3"><input type="text" class="form-control" name="others" v-model="cultureBiopsyData.others" id="others"></td>
              <td colspan="2">
                <select class="form-control ls-select2" name="cytology_01" v-validate="'required'" v-model="cultureBiopsyData.cytology_01"  value="">
                  <option value="myco tb">Myco TB</option>
                  <option value="aerobic">Aerobic</option>
                  <option value="anaerobic">Anaerobic</option>
                </select>
                <span class="help is-danger" v-show="errors.has('cytology_01')">
                  Field is required
                </span>
              </td>
            </tr>
            <tr>
              <td colspan="2">
                <select class="form-control ls-select2" name="histopathology" v-validate="'required'" v-model="cultureBiopsyData.histopathology"  value="">
                  <option value="gram stain">Gram Stain</option>
                  <option value="zn stain">ZN Stain</option>
                </select>
                <span class="help is-danger" v-show="errors.has('histopathology')">
                  Field is required
                </span>
              </td>
              <td></td>
              <td></td>
            </tr>


  				</tbody>
  		  </table>

  		  <table class="table table-bordered">
  		    <thead>
    			  <tr>
    				<th>Type of Biopsy</th>
    				<th>Specimen</th>
    			  </tr>
    			</thead>
    			<tbody>
    			  <tr>
    			    <td class="form-inline">
    			    	<label class="control-label" for="type_of_biopsy">Surgical Excision:</label>
    						<div class="">
    							<select class="form-control ls-select2" name="type_of_biopsy" v-validate="'required'" v-model="cultureBiopsyData.type_of_biopsy"  value="">
    								<option value="complete">Complete</option>
    								<option value="partial">Partial</option>
    								<option value="punch">Punch</option>
    								<option value="needle">Needle</option>
    								<option value="endoscopic">Endoscopic</option>
    							</select>
                  <span class="help is-danger" v-show="errors.has('type_of_biopsy')">
                    Field is required
                  </span>
    						</div>
    			    </td>
      				<td>
      					<input class="form-control ls-select2" type="text" name="specimen_info" v-validate="'required'" v-model="cultureBiopsyData.specimen_info" placeholder="Specimen info here" />
                <span class="help is-danger" v-show="errors.has('specimen_info')">
                  Field is required
                </span>
      				</td>
    			  </tr>
    			</tbody>
  		  </table>
  		  <hr>
        <div class="row">
		  		<label class="control-label" for="clinical_details"><h4>Patient's Clinical Details:</h4></label>
					<textarea class="form-control" rows="6" name="clinical_details" v-validate="'required'" v-model="cultureBiopsyData.clinical_details" id="clinical_details" placeholder="Enter the patient's clinical details"></textarea>
          <span class="help is-danger" v-show="errors.has('clinical_details')">
            Field is required
          </span>
  		  </div>

        <hr>

        <div class="row">
		  		<label class="control-label" for="imaging_details"><h4>Imaging Details:</h4></label>
					<textarea class="form-control" rows="6" name="imaging_details" v-validate="'required'" v-model="cultureBiopsyData.imaging_details" id="imaging_details" placeholder="Graph?"></textarea>
          <span class="help is-danger" v-show="errors.has('imaging_details')">
            Field is required
          </span>
  		  </div>
  		  <hr>
  		  <div class="row form-group">
          <div class="col-md-6">
            <label>Send To : </label>
          </div>
		  		<div class="col-md-6">
						<select class="form-control ls-select2" name="send_to" v-model="cultureBiopsyData.send_to" value="">
							<option value="toprani lab">Toprani Lab</option>
							<option value="kadakiya lab">Kadakiya Lab</option>
						</select>
		  		</div>
  		  </div>

        <div class="row form-group">
          <div class="col-md-6">
            <div class="col-md-6">
              <label>Consultant's Sign:</label>
            </div>
            <div class="col-md-6">
              <input type="text" class="form-control" name="consultant_sign" v-model="cultureBiopsyData.consultant_sign" id="consultant_sign">
            </div>
          </div>
          <div class="col-md-6">
            <div class="col-md-6">
              <label>Consultant's Name:</label>
            </div>
            <div class="col-md-6">
              <input type="text" class="form-control" name="consultant_name" v-validate="'required'" v-model="cultureBiopsyData.consultant_name" id="consultant_name">
              <span class="help is-danger" v-show="errors.has('consultant_name')">
                Field is required
              </span>
            </div>
          </div>
        </div>

        <div class="row form-group">
          <div class="col-md-6">
            <div class="col-md-6">
              <label>Date:</label>
            </div>
            <div class="col-md-6">
              <input type="date" class="form-control " name="date_for_sign" v-validate="'required'" v-model="cultureBiopsyData.date_for_sign" id ="date_for_sign">
              <span class="help is-danger" v-show="errors.has('date_for_sign')">
                Field is required
              </span>
            </div>
          </div>
          <div class="col-md-6">
            <div class="col-md-6">
              <label>Time:</label>
            </div>
            <div class="col-md-6">
              <input type="time" class="form-control " name="time_for_sign" v-validate="'required'" v-model="cultureBiopsyData.time_for_sign" id ="time_for_sign">
              <span class="help is-danger" v-show="errors.has('time_for_sign')">
                Field is required
              </span>
            </div>
          </div>
        </div>

  			<div class="text-center form-group">
  				<button class="btn btn-success" type="button" @click="saveCultureBiopsy()">Submit</button>
  			</div>
  	</form>
       <select-patient-modal @confirmed="deleteConfirmed()"></select-patient-modal>
  </div>
</template>
<script >
	import User from '../../../api/users.js';
  import addressograph from './addressograph.vue';
  import SelectPatientModal from '../../../components/SelectPatientModal.vue';

    export default {
        data() {
            return {
                'footer' : 'footer',
                'currentYear': new Date().getFullYear(),
                'type': 'cultureBiopsyForm',
                'patient_id': this.$store.state.Patient.patientId,
               	'ipd_id': this.$store.state.Patient.ipdId,
                'cultureBiopsyData' : {
                  'date_culture' : '',
                  'time_culture' : '',
                  'doctors_name' : '',
                  'given_by' : '',
                  'received_by' : '',
                  'ihc' : '',
                  'cytology' : '',
                  'others' : '',
                  'histopathology' : '',
                  'cytology_01' : '',
                  'type_of_biopsy' : '',
                  'specimen_info' : '',
                  'clinical_details' : '',
                  'imaging_details' : '',
                  'send_to' : '',
                  'consultant_sign' : '',
                  'consultant_name' : '',
                  'date_for_sign' : '',
                  'time_for_sign' : '',
                }
            }
        },
        components: {
           addressograph,
           SelectPatientModal
       },
       mounted() {
         $('.ls-select2').select2({
            placeholder: "Select",
         });
         if(this.ipd_id == 0){
                $('#delete_modal').modal('show');
          }
         $('.ls-datepicker').datepicker({
				    format: 'dd/mm/yyyy',
				    'autoclose': true
          })
            let vm =this;
              $('.ls-datepicker').datepicker({
                 format: 'dd/mm/yyyy',
                  'autoclose': true
              });
              $('.ls-timepicker').timepicker({
                format: 'hh-mm',
                'autoclose': true
              });
            $('.ls-datepicker').datepicker().on('changeDate',function(){
     					if (this.id == 'date') {
     						vm.cultureBiopsyData.date_culture = this.value;
     					}
              if (this.id == 'date_for_sign') {
               vm.cultureBiopsyData.date_for_sign = this.value;
              }
     			  })

            $('.ls-timepicker').timepicker().on('change',function(){
               if (this.id == 'time') {
                 vm.cultureBiopsyData.time_culture = this.value;
               }
               if (this.id == 'time_for_sign') {
                 vm.cultureBiopsyData.time_for_sign = this.value;
               }
            });

				},
        methods: {
		    GetSelectComponent(componentName) {
		       this.$router.push({name: componentName})
		    },
		    saveCultureBiopsy() {
		    	this.$validator.validateAll().then(
	            (response) => {
	            	if (!this.errors.any()) {
	            		 $("body .js-loader").removeClass('d-none');
                   var cultureBiopsyData = {'type':this.type,'patient_id':this.patient_id,'ipd_id':this.ipd_id,'form_data':this.cultureBiopsyData};
				    	User.saveCultureBiopsy(cultureBiopsyData).then(
		                (response) => {
		                	if(response.data.status == 200) {
		                		toastr.success('Culture Biopsy details have been saved', 'Culture Biopsy Form', {timeOut: 5000});
		                	}
		                	 $("body .js-loader").addClass('d-none');

		                },
		                (error) => {
		                	 $("body .js-loader").addClass('d-none');

		                }
		                )
			    	}
			    },
                (error) => {
                }
                )

			}
		  },

    }
</script>
