<template>
    <div>
        <sidebar-component></sidebar-component>
        <nav-mobile />
        <div class="page">
            <button class="btn mt-3 back" onclick="javascript:window.history.back();">
                <font-awesome-icon icon="fa-solid fa-chevron-left" />
                Go back
            </button>
            <!-- section profile atas -->
            <section class="section-profile">
                <div class="row">
                    <div class="col-md-3">
                        <img v-if="applicant.jobseekerImage != null" :src="applicant.jobseekerImage" alt="profile picture"
                            height="200" max-width="250">
                        <img v-else src="https://toptalentapp.com:9091/resources/r5jr7e3qf8f5uhr.png" alt="profile picture"
                            height="200" width="200">
                    </div>
                    <div class="col-md-6 middle-item">
                        <h1>{{ applicant.jobseekerName }}</h1>
                        <div>
                            <h2 class="mt-3" v-if="applicant.jobseekerProfession != null">{{ applicant.jobseekerProfession
                            }}
                            </h2>
                            <h2 v-else></h2>
                        </div>

                        <div class="mt-2">
                            <p
                                v-if="applicant.workEndYear == 0 || applicant.workStartYear == 0 || applicant.jobsekerCompany == null">
                            </p>

                            <p v-else class="jobseeker-company">{{ applicant.jobsekerCompany }} ({{ applicant.workStartYear
                            }} -
                                {{ applicant.workEndYear }})</p>
                        </div>
                        <div class="detail-profile">
                            <div class="row">
                                <div class="col-md-6 left-items ">
                                    <div>
                                        <p
                                            v-if="applicant.cityName == null || applicant.cityName == '' || applicant.countryName == null || applicant.countryName == ''">
                                            <font-awesome-icon icon="fa-solid fa-location-dot"
                                                class="detail-profile-icon" /> -
                                        </p>
                                        <p v-else><font-awesome-icon icon="fa-solid fa-location-dot"
                                                class="detail-profile-icon" /> &nbsp;{{ applicant.cityName }},{{
                                                    applicant.countryName }}</p>
                                    </div>

                                    <p v-if="applicant.jobseekerPhone != null"><font-awesome-icon icon="fa-solid fa-phone"
                                            class="detail-profile-icon" /> {{ applicant.jobseekerPhone }}</p>
                                    <p v-if="applicant.jobseekerPhone == null"><font-awesome-icon icon="fa-solid fa-phone"
                                            class="detail-profile-icon" /> -</p>
                                </div>
                                <div class="col-md-5 right-items">
                                    <div>
                                        <p
                                            v-if="applicant.jobseekerProfession == null || applicant.jobseekerProfession == ''">
                                            <font-awesome-icon icon="fa-regular fa-circle" class="detail-profile-icon" /> -
                                        </p>
                                        <p v-else><font-awesome-icon icon="fa-regular fa-circle"
                                                class="detail-profile-icon" /> {{ applicant.jobseekerProfession }}</p>
                                    </div>

                                    <div>
                                        <p v-if="applicant.jobseekerEmail == null || applicant.jobseekerEmail == ''">
                                            <font-awesome-icon icon="fa-regular fa-envelope" class="detail-profile-icon" />
                                            -
                                        </p>
                                        <p v-else><font-awesome-icon icon="fa-regular fa-envelope"
                                                class="detail-profile-icon" /> {{ applicant.jobseekerEmail }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <a target="_blank" v-bind:href="applicant.jobseekerResume" type="application/octet-stream"
                            download="" class="btn btn-primary btn-resume">
                            <font-awesome-icon icon="fa-solid fa-cloud-arrow-down" />
                            Resume
                        </a>

                        <div class="mt-4">
                            <button v-if="applicant.jobseekerPortfolio == '' || applicant.jobseekerPortfolio == null"
                                hidden></button>
                            <button class="btn-porto" v-else
                                v-on:click="getLink(applicant.jobseekerPortfolio)"><font-awesome-icon
                                    icon="fa-solid fa-link" class="icon-background" /> Portfolio</button>
                        </div>
                    </div>
                </div>
            </section>
            <section class="section-content">
                <div class="row">
                    <div class="col-md-5 content-left">
                        <h3 class="mb-4"><strong>Educational Background</strong></h3>
                        <div v-for="(item, index) in applicant.jobseekerEducation" v-bind:key="index" class="d-flex">
                            <div class="col-1">
                                <font-awesome-icon icon="fa-solid fa-graduation-cap " class="icon-background" />
                            </div>
                            <div class="col-11 background-detail">
                                <h5 class="mb-2"><strong>{{ item.schoolUniversity }}</strong> </h5>
                                <p>{{ item.degreeName }}, {{ item.majorName }} </p>
                                <p>{{ item.startPeriodMonth }}, {{ item.startPeriodYear }} - {{ item.endPeriodMonth }}, {{
                                    item.endPeriodYear }} </p>
                                <p>GPA : {{ item.gradePoint.toFixed(2) }} / {{ item.gradePointMax.toFixed(2) }}</p>
                                <p v-if="educationDescription">{{ item.educationDescription }}</p>
                                <p v-else></p>
                            </div>
                        </div>

                        <h3 class="mb-4 mt-4"><strong>Working Experiences</strong></h3>
                        <div v-for="(item, index) in applicant.jobseekerExperience" v-bind:key="index" class="d-flex">
                            <div class="col-1">
                                <font-awesome-icon icon="fa-solid fa-suitcase" class="icon-background" />
                            </div>
                            <div class="col-11 background-detail">
                                <h5 class="mb-2"> <strong>{{ item.jobTitle }}</strong> </h5>
                                <p>{{ item.jobFunction }} </p>
                                <p>{{ item.companyName }} </p>
                                <p>{{ item.jobType }} </p>
                                <p>{{ item.cityName }}, {{ item.countryName }} </p>
                                <p v-if="item.isPresent == '0'">{{ item.startPeriodMonth }}, {{ item.startPeriodYear }} - {{
                                    item.endPeriodMonth }}, {{ item.endPeriodYear }} </p>
                                <p v-else>{{ item.startPeriodMonth }}, {{ item.startPeriodYear }} - Present</p>
                                <p>{{ item.jobDescription }}</p>
                            </div>
                        </div>

                        <div>
                            <p v-if="applicant.jobseekerDateOfBirth != null"><font-awesome-icon
                                    icon="fa-solid fa-cake-candles" class="icon-background" />
                                {{ applicant.jobseekerDateOfBirth }}</p>
                            <p v-else></p>
                        </div>



                        <div>
                            <a href="#" v-if="applicant.jobseekerMedsos == null || applicant.jobseekerMedsos == ''"></a>
                            <a v-bind:href="applicant.jobseekerMedsos" target="_blank" v-else><font-awesome-icon
                                    icon="fa-solid fa-user-group" class="icon-background" /> LinkedIn</a>
                        </div>
                    </div>
                    <div class="col-md-7">
                        <h3>Skills</h3>
                        <div class="box-skills d-flex flex-wrap">
                            <!-- <span class="badge text-dark" :key="item.id" v-for="item in splitStringToArray(applicant.jobseekerSkill)">{{item}}</span> -->
                            <span class="badge text-dark" v-for="item in (skill)" :key="item.id">{{ item }}</span>

                        </div>
                    </div>
                </div>
                <section class="section-about">
                    <div>
                        <h3 v-if="applicant.jobseekerAbout == '' || applicant.jobseekerAbout == null"></h3>
                        <h3 v-else>About</h3>

                    </div>
                    <div class="box-about pb-5">
                        <p>
                            {{ applicant.jobseekerAbout }}
                        </p>
                    </div>
                </section>

            </section>

        </div>
    </div>
</template>

<script>
import sidebarcomponent from '../components/SidebarComponent.vue'
import NavMobile from '../components/NavMobile.vue'
import axios from 'axios'

export default {
    name: "ApplicantDetailView",
    components: {
        SidebarComponent: sidebarcomponent,
        NavMobile: NavMobile,
    },
    data() {
        return {
            applicant: "",
            skill: [],
            item: ''
        }
    },
    methods: {
        splitStringToArray(sentences) {
            const words = sentences.split(';');
            console.log(words);
            return words;

        },
        // get data form api
        async fetchData() {
            try {
                await axios.get(`https://toptalentapp.com:9091/api/v1/jobseeker/user/ ` + this.$route.params.id)
                    // createToast("Job Updated", { type: "success" });
                    .then((data) => {
                        this.applicant = data.data.data
                        console.log(data.data.data)
                    })
            } catch {
                console.log(Error);
            }

        },
        async getStringSkill() {
            try {
                await axios.get(`https://toptalentapp.com:9091/api/v1/jobseeker/user/ ` + this.$route.params.id)
                    .then((data) => {
                        this.skill = data.data.data.skills.map(({ skillName }) => skillName)
                        // this.name = this.skill.map(({skillName}) => skillName)
                        console.log(this.skill);
                        // console.log(this.name);
                    })
            } catch {
                console.log(Error);
            }

        },
        async getLink(jobseekerPortofolio) {
            window.open(`${jobseekerPortofolio}`);
        },
    },
    mounted() {

        this.fetchData();
        this.getStringSkill();
    },

}
</script>

<style scoped>
* {
    padding: 0;
    margin: 0;
}

.page {
    margin-left: 20%;
}

.section-profile {
    margin-top: 50px;
}


.section-profile .middle-item {
    padding-left: 20px;
}

.section-profile h1 {
    font-size: 28px;
}

.section-profile h2 {
    font-size: 16px;
}

.section-profile h3 {
    font-size: 16px;
    margin-top: 10px;
}

.section-profile .detail-profile {
    margin-top: 70px;
}

.detail-profile p {
    margin-bottom: 10px;
}

.detail-profile-icon {
    color: #006EFF;
}

.icon-background {
    color: #006EFF;
}

.right-items {
    margin-left: -80px;
}

.btn-resume {
    padding: 10px;
}

.section-content {
    background-color: #F8F9FD;
    margin-top: 26px;
    padding-left: 70px;
    padding-top: 30px;
    margin-left: -5%;
}

.content-left p {
    margin-bottom: 20px;
}

.content-left a {
    display: block;
    margin-bottom: 20px;
    text-decoration: none;
    color: #000;
}

.content-left a:hover {
    color: #006EFF;
    text-decoration: underline;
}

.content-left h5 {
    font-size: 17px;
}

.content-left .background-detail p {
    margin: 0.1em;
    padding: 0.1em;
}

h3 {
    font-size: 20px;
}

.box-skills {
    max-width: 410px;
    max-height: 225px;
    overflow-y: scroll;
}

.box-skills span {
    margin: 5px;
    padding: 10px;
    font-size: 14px;
    background-color: #D1E7F3;
}

.section-about {
    max-width: 930px;
    margin-top: 70px;
    margin-bottom: 80px;

}

.section-about .box-about {
    margin-top: 30px;
}

.section-about h3 {
    font-weight: 600;
}

.box-about p {
    font-size: 20px;
}

.jobseeker-company {
    font-size: 12px !important;
}

.btn-porto {
    border: none;
    background-color: transparent;
    margin-bottom: 20px;
}

.btn-porto:hover {
    text-decoration: underline;
    color: #006EFF;
}

/*css3 design scrollbar*/
::-webkit-scrollbar {
    width: 5px;
}

::-webkit-scrollbar-track {
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    background: #F8F9FA;
}

::-webkit-scrollbar-thumb {
    background: #006EFF;
}

/* breakpoints */
/* for mobile */
@media only screen and (max-width: 576px) {
    .page {
        margin-top: 60px;
        margin-left: 10%;
    }

    .right-items {
        margin-left: 0;
    }

    h1 {
        margin-top: 20px;
    }

}
</style>