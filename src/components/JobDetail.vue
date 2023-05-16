<template>
  <div>
    <sidebarcomponent />
    <nav-mobile />
    <div class="main">
      <div class="col-md-11">

        <div class="candidates p-4 mb-4">

          <h5 class="fw-bold">Candidates</h5>

          <div class="row justify-content-between mt-4">
            <div class="col-6 d-flex align-items-center">
              <h6 class="fw-bold">{{ job.jobName }}</h6>
            </div>

            <div class="col-6 d-flex justify-content-end align-items-center">

              <router-link :to="{ name: 'jobsdetail', params: { id: job.jobId } }">
                <img class="import-icon" src="../assets/icon-postjob/see-all.svg" alt="">
              </router-link>

              <button class="ict prm" data-bs-toggle="modal" :data-bs-target="'#exampleModalToggle' + job.jobId"
                role="button" v-on:click="getDetail(job.jobId)">
                <img class="import-icon" src="../assets/icon-postjob/edit.svg" alt="">
              </button>

              <div class="modal fade" :id="'exampleModalToggle' + job.jobId" aria-hidden="true"
                aria-labelledby="exampleModalToggleLabel" tabindex="-1">
                <div class="modal-dialog modal-xl">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h5 class="modal-title" id="exampleModalToggleLabel">Edit Job</h5>
                      <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                      <form @submit.prevent="submitForm">
                        <div class="mb-3">
                          <label for="recipient-name" class="col-form-label">Job Name:</label>
                          <input type="text" class="form-control" id="" v-model="edit.jobName">
                        </div>
                        <div class="mb-3">
                          <label for="recipient-name" class="col-form-label">Job salary:</label>
                          <input type="number" class="form-control" v-model="edit.jobSalary" min="1" step="1"
                            onkeypress="return event.keyCode === 8 || event.charCode >= 48 && event.charCode <= 57"
                            maxlength="8" @keyup="limitInput" required>
                        </div>
                        <div class="mb-3">
                          <label for="recipient-name" class="col-form-label">Job position: </label>
                          <select class="form-control" id="inputState" v-model="edit.jobPosition" required>
                            <option selected disabled>Choose..</option>
                            <option>Internship</option>
                            <option>Full time</option>
                            <option>Part time</option>
                            <option>Contractual</option>
                            <option>Freelance</option>
                          </select>
                        </div>
                        <div class="mb-3">
                          <label for="recipient-name" class="col-form-label">Job Requirement: </label>
                          <!-- <input type="text" class="form-control" id="recipient-name" v-model="edit.jobRequirement"> -->
                          <ckeditor :editor="editor" tag-name="textarea" :model-value="jobDesc"
                            v-model="edit.jobRequirement" :config="editorConfig"></ckeditor>
                        </div>
                        <div class="mb-3">
                          <label for="message-text" class="col-form-label">Job Description:</label>
                          <!-- <textarea class="form-control" id="message-text" v-model="edit.jobDesc" /> -->
                          <ckeditor :editor="editor" tag-name="textarea" :model-value="jobDesc" v-model="edit.jobDesc"
                            :config="editorConfig"></ckeditor>
                        </div>
                        <div class="mb-3">
                          <label for="recipient-name" class="col-form-label">Job Address: </label>
                          <input type="text" class="form-control" id="recipient-name" v-model="edit.jobAddress"
                            maxlength="100" required>
                        </div>
                        <div class="modal-footer">
                          <button class="btn btn-success" type="submit">Update</button>
                        </div>
                      </form>
                    </div>
                  </div>
                </div>
              </div>

              <button class="ict dgr icon-delete" v-on:click="deleteJob(job.jobId)">
                <img class="import-icon" src="../assets/icon-postjob/delete.svg" alt="">
              </button>
            </div>
            <div class="col-12">
              <!-- <small class="text-muted">Created on {{job.createdAt}}</small> -->
              <small class="text-muted">Created on {{ moment(job.createdAt).format('DD MMM YYYY') }}</small>
            </div>
          </div>

        </div>

        <div class="applicantDetail">
          <ul class="nav nav-tabs nav-fill" id="myTab" role="tablist">
            <li class="nav-item" role="presentation">
              <button class="nav-link active" id="unreview-tab" data-bs-toggle="tab" data-bs-target="#unreview"
                type="button" role="tab" aria-controls="unreview" aria-selected="true">Unreviewed</button>
            </li>
            <li class="nav-item" role="presentation">
              <button class="nav-link" id="accept-tab" data-bs-toggle="tab" data-bs-target="#accept" type="button"
                role="tab" aria-controls="accept" aria-selected="false">Accepted</button>
            </li>
            <li class="nav-item" role="presentation">
              <button class="nav-link" id="reject-tab" data-bs-toggle="tab" data-bs-target="#reject" type="button"
                role="tab" aria-controls="reject" aria-selected="false">Rejected</button>
            </li>
          </ul>
          <!-- CANDIDATE  -->
          <div class="tab-content">
            <div class="tab-pane active" id="unreview" role="tabpanel" aria-labelledby="unreview-tab">
              <div v-for="item in list" v-bind:key="item.id">
                <applicantjobcomponent :item="item" v-if="item.applicationStatus == 'sent'"></applicantjobcomponent>
              </div>
            </div>
            <div class="tab-pane" id="accept" role="tabpanel" aria-labelledby="accept-tab">
              <div v-for="item in list" v-bind:key="item.id">
                <applicantjobcomponent :item="item" v-if="item.applicationStatus == 'accepted'"></applicantjobcomponent>
              </div>
            </div>
            <div class="tab-pane" id="reject" role="tabpanel" aria-labelledby="reject-tab">
              <div v-for="item in list" v-bind:key="item.id">
                <applicantjobcomponent :item="item" v-if="item.applicationStatus == 'rejected'"></applicantjobcomponent>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';
import axios from 'axios'
import sidebarcomponent from '@/components/SidebarComponent.vue'
import applicantjobcomponent from '@/components/ApplicantJobComponent.vue'
import NavMobile from '@/components/NavMobile.vue'
import moment from 'moment';
moment().format();
export default {
  props: ['item'],
  components: {
    sidebarcomponent,
    applicantjobcomponent,
    NavMobile,
  },
  data() {
    return {
      editor: ClassicEditor,
      editorData: '',
      editorConfig: {
        // The configuration of the editor.
        toolbar: {
          items: [
            'heading',
            '|',
            'bold',
            'italic',
            'bulletedList',
            'undo',
            'redo'
          ]
        }
      },
      job: [],
      list: [],
      edit: []

    }
  },
  methods: {
    moment: function (date) {
      return moment(date);
    },
    submitForm() {
      if (this.edit.jobRequirement.trim() === '') {
        alert('Please fill Job Requirement!');
        return;
      }
      if (this.edit.jobDesc.trim() === '') {
        alert('Please fill Job description!');
        return;
      }

      // call your updateJobData method with the editor content
      this.updateJobData(this.edit.jobId, this.edit.jobDesc, this.edit.jobRequirement);
    },

    limitInput() {
      if (this.edit.jobSalary.length > 8) {
        this.edit.jobSalary = this.edit.jobSalary.slice(0, 8);
      }
    },

    async getJobDetail() {
      await axios.get(`https://toptalentapp.com:9091/api/v1/job/` + this.$route.params.id)
        .then((data) => {
          this.job = data.data.data
          console.log('jobdetail')
          console.log(this.job)
        })
    },
    async getCandidate() {
      await axios.get(`https://toptalentapp.com:9091/api/v1/application/applicants/` + this.$route.params.id)
        .then((data) => {
          this.list = data.data
          console.log('candidate')
          console.log(this.list)
        })
    },
    async deleteJob(jobId) {
      try {
        let result = await axios.put(`https://toptalentapp.com:9091/api/v1/job/delete/` + jobId);
        console.warn(result)
        //  createToast("Job Deleted!", { type: "danger" });
        this.$toast.success('Job deleted')
        location.reload(true)
        window.location = "/postjobnew";
      } catch {
        console.warn
      }
    },
    async updateJobData(id) {
      try {
        await axios.patch(
          `https://toptalentapp.com:9091/api/v1/job/${id}?jobName=${encodeURIComponent(this.edit.jobName)}&jobStatus=${encodeURIComponent(this.edit.jobStatus)}&jobSalary=${encodeURIComponent(this.edit.jobSalary)}&jobPosition=${encodeURIComponent(this.edit.jobPosition)}&jobAddress=${encodeURIComponent(this.edit.jobAddress)}&jobDesc=${encodeURIComponent(this.edit.jobDesc)}&jobRequirement=${encodeURIComponent(this.edit.jobRequirement)}`
        )
        // createToast("Job Updated", { type: "success" });
        location.reload(true)
      } catch {
        // console.log(warn)
      }
    },
    async getDetail(id) {
      try {
        console.log(id)
        await axios.get(`https://toptalentapp.com:9091/api/v1/job/${id}`)
          .then((data) => {
            this.edit = data.data.data
            console.log(data.data)
          })
      } catch {
        console.log(Error)
      }
    },


  },
  mounted() {
    this.getDetail();
    this.getJobDetail();
    this.getCandidate();
  }
}
</script>

<style scoped>
.main {
  margin-left: 280px;
}

.back {

  border-radius: 30px;
  height: fit-content;

}

.back:hover {

  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.icon-delete {
  margin-left: -1px;
}

/* BREAKPOINTS */
/* MOBILE */
@media only screen and (max-width: 576px) {
  .main {
    margin-left: 0;
    margin-top: 60px;
  }

  .back {
    margin-left: 10px;
  }
}
</style>