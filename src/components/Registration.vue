<template>
    <form action="#" method="post" class="steps">
        <fieldset id="step_1">

            <legend>Step 1</legend>
            <label for="num_attendees">
                How many people will be attending?
            </label>
            <select id="num_attendees" @change="onSelectAttendee($event)">
                <option id="opt_0" value="0">Please Choose</option>
                <option id="opt_1" value="1">1</option>
                <option id="opt_2" value="2">2</option>
                <option id="opt_3" value="3">3</option>
                <option id="opt_4" value="4">4</option>
                <option id="opt_5" value="5">5</option>
            </select>
            <br>
            <div id="attendee_container">
                <div id="attendee_1_wrap" v-if="showAttendee(1)">
                    <h3>Please provide full names:</h3>
                    <label for="name_attendee_1">
                        Attendee 1 Name:
                    </label>
                    <input type="text" id="name_attendee_1" @keyup="onCompileAttendee($event, 1)">
                </div>
                <div id="attendee_2_wrap" v-if="showAttendee(2)">
                    <label for="name_attendee_2">
                        Attendee 2 Name:
                    </label>
                    <input type="text" id="name_attendee_2" @keyup="onCompileAttendee($event, 2)">
                </div>
                <div id="attendee_3_wrap" v-if="showAttendee(3)">
                    <label for="name_attendee_3">
                        Attendee 3 Name:
                    </label>
                    <input type="text" id="name_attendee_3" @keyup="onCompileAttendee($event, 3)">
                </div>
                <div id="attendee_4_wrap" v-if="showAttendee(4)">
                    <label for="name_attendee_4">
                        Attendee 4 Name:
                    </label>
                    <input type="text" id="name_attendee_4" @keyup="onCompileAttendee($event, 4)">
                </div>
                <div id="attendee_5_wrap" v-if="showAttendee(5)">
                    <label for="name_attendee_5">
                        Attendee 5 Name:
                    </label>
                    <input type="text" id="name_attendee_5" @keyup="onCompileAttendee($event, 5)">
                </div>
                <div id="step1_result" v-if="step1">
                    <img alt="check" src="/assets/check-ok.png">
                </div>
            </div>

        </fieldset>


        <fieldset id="step_2">
            <div class="overlay" v-if="!step1"></div>
            <legend>Step 2</legend>
            <p>
                Would you like your company name on your badges?
            </p>
            <input type="radio" id="company_name_toggle_on" name="company_name_toggle_group"
                   @change="changeCompany(true)" :disabled="!step1">
            <label for="company_name_toggle_on">Yes</label>
            &emsp;
            <input type="radio" id="company_name_toggle_off" name="company_name_toggle_group"
                   @change="changeCompany(false)" :disabled="!step1">
            <label for="company_name_toggle_off">No</label>
            <div id="company_name_wrap" v-if="company">
                <label for="company_name">
                    Company Name:
                </label>
                <input type="text" id="company_name" v-model="companyName" @keyup="updateStep2">
            </div>
            <div>
                <p>
                    Will anyone in your group require special accommodations?
                </p>
                <input type="radio" id="special_accommodations_toggle_on" name="special_accommodations_toggle"
                       @change="changeAccomodation(true)">
                <label for="special_accommodations_toggle_on">Yes</label>
                &emsp;
                <input type="radio" id="special_accommodations_toggle_off" name="special_accommodations_toggle"
                       @change="changeAccomodation(false)">
                <label for="special_accommodations_toggle_off">No</label>
            </div>
            <div id="special_accommodations_wrap" v-if="accomodation">
                <label for="special_accomodations_text">
                    Please explain below:
                </label>
                <textarea rows="10" cols="10" id="special_accomodations_text" v-model="accomodationCompile"
                          @keyup="updateStep2"></textarea>
            </div>

            <div id="step2_result" v-if="step2">
                <img alt="check" src="/assets/check-ok.png">
            </div>

        </fieldset>


        <fieldset id="step_3">
            <div class="overlay" v-if="!step2"></div>
            <legend>Step 3</legend>
            <label for="rock">
                Are you ready to rock?
            </label>
            <input type="checkbox" id="rock" v-model="step3" :disabled="!step2">
            <input type="submit" id="submit_button" value="Complete Registration" :disabled="!step3">
        </fieldset>
    </form>
</template>

<script>
    export default {
        name: "Registration",

        data: function () {
            return {
                attendeeNum: 0,
                attendeeCompiled: [],
                step1: false,
                step2: false,
                step3: false,
                company: null,
                companyName: '',
                accomodation: null,
                accomodationCompile: '',
            }
        },

        methods: {
            showAttendee(item) {
                return this.attendeeNum >= item;
            },
            onSelectAttendee(event) {
                this.attendeeNum = event.target.value;
            },
            onCompileAttendee(event, id) {
                this.attendeeCompiled[id] = event.target.value;
                for (let i = 1; i <= this.attendeeNum; i++) {
                    if (this.attendeeCompiled[i] === undefined || this.attendeeCompiled[i] === '')
                        return;
                }
                this.step1 = true;

            },
            changeCompany(val) {
                this.company = val;
                this.updateStep2();
            },
            changeAccomodation(val) {
                this.accomodation = val;
                this.updateStep2();
            },
            updateStep2() {
                this.step2 = (this.company ? (this.companyName !== '') : this.company != null) && (this.accomodation ? (this.accomodationCompile !== '') : this.accomodation !== null);
            }

        }

    }
</script>

<style scoped>
    .steps {
        display: flex;
        padding: 50px;
        background-color: #3f3f40;
    }

    legend {
        background-color: white;
        border-radius: 5px;
        padding: 5px;
        position: absolute;
        margin-top: -30px;
    }

    .overlay {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.5);
    }

    #step_1 {
        background-color: #8eb9b2;
        position: relative;
    }

    #step_2 {
        background-color: #2c9cb9;
        position: relative;

    }

    #step_3 {
        background-color: #b9724c;
        position: relative;

    }
</style>
