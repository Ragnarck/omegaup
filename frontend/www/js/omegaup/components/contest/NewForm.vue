<template>
  <div class="card panel panel-primary">
    <div class="card-header bg-primary text-white panel-heading" v-if="!update">
      <h3 class="panel-title">{{ T.contestNew }}</h3>
    </div>
    <div class="card-body panel-body">
      <div class="btn-group bottom-margin mb-3">
        <button class="btn btn-default btn-secondary" v-on:click="fillOmi">
          {{ T.contestNewFormOmiStyle }}
        </button>
        <button class="btn btn-default btn-secondary" v-on:click="fillPreIoi">
          {{ T.contestNewForm }}
        </button>
        <button class="btn btn-default btn-secondary" v-on:click="fillConacup">
          {{ T.contestNewFormConacupStyle }}
        </button>
        <button class="btn btn-default btn-secondary" v-on:click="fillIcpc">
          {{ T.contestNewFormICPCStyle }}
        </button>
      </div>
      <form class="contest-form" v-on:submit.prevent="onSubmit">
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.wordsTitle }}</label>
            <input
              class="form-control"
              name="title"
              data-title
              v-bind:placeholder="titlePlaceHolder"
              size="30"
              type="text"
              v-model="title"
            />
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormShortTitle_alias_ }}</label>
            <input
              class="form-control"
              name="alias"
              v-bind:disabled="update"
              type="text"
              v-model="alias"
            />
            <p class="help-block">
              {{ T.contestNewFormShortTitle_alias_Desc }}
            </p>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormStartDate }}</label>
            <omegaup-datetimepicker
              v-model="startTime"
            ></omegaup-datetimepicker>
            <p class="help-block">{{ T.contestNewFormStartDateDesc }}</p>
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormEndDate }}</label>
            <omegaup-datetimepicker
              v-model="finishTime"
            ></omegaup-datetimepicker>
            <p class="help-block">{{ T.contestNewFormEndDateDesc }}</p>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormDescription }}</label>
            <textarea
              class="form-control"
              name="description"
              cols="30"
              rows="10"
              v-model="description"
            ></textarea>
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormDifferentStarts }}</label>
            <div class="checkbox">
              <label
                ><input type="checkbox" v-model="windowLengthEnabled" />
                {{ T.wordsEnable }}</label
              >
            </div>
            <input
              class="form-control"
              size="3"
              type="text"
              v-bind:disabled="!windowLengthEnabled"
              v-model="windowLength"
            />
            <p class="help-block">{{ T.contestNewFormDifferentStartsDesc }}</p>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormScoreboardTimePercent }}</label>
            <input
              class="form-control scoreboard-time-percent"
              name="scoreboard"
              size="3"
              type="text"
              v-model="scoreboard"
            />
            <p class="help-block">
              {{ T.contestNewFormScoreboardTimePercentDesc }}
            </p>
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormSubmissionsSeparation }}</label>
            <input
              class="form-control"
              size="2"
              type="text"
              v-model="submissionsGap"
            />
            <p class="help-block">
              {{ T.contestNewFormSubmissionsSeparationDesc }}
            </p>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormPenaltyType }}</label>
            <select class="form-control" v-model="penaltyType">
              <option value="none">
                {{ T.contestNewFormNoPenalty }}
              </option>
              <option value="problem_open">
                {{ T.contestNewFormByProblem }}
              </option>
              <option value="contest_start">
                {{ T.contestNewFormByContests }}
              </option>
              <option value="runtime">
                {{ T.contestNewFormByRuntime }}
              </option>
            </select>
            <p class="help-block">{{ T.contestNewFormPenaltyTypeDesc }}</p>
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.wordsPenalty }}</label>
            <input
              class="form-control"
              size="2"
              type="text"
              v-model="penalty"
            />
            <p class="help-block">{{ T.contestNewFormPenaltyDesc }}</p>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-4">
            <label>{{ T.wordsFeedback }}</label>
            <select class="form-control" v-model="feedback">
              <option value="none">
                {{ T.wordsNone }}
              </option>
              <option value="summary">
                {{ T.wordsSummary }}
              </option>
              <option value="detailed">
                {{ T.wordsDetailed }}
              </option>
            </select>
            <p class="help-block">
              {{ T.contestNewFormImmediateFeedbackDesc }}
            </p>
          </div>
          <div class="form-group col-md-4">
            <label>{{ T.contestNewFormScoreboardAtEnd }}</label>
            <select class="form-control" v-model="showScoreboardAfter">
              <option v-bind:value="true">
                {{ T.wordsYes }}
              </option>
              <option v-bind:value="false">
                {{ T.wordsNo }}
              </option>
            </select>
            <p class="help-block">{{ T.contestNewFormScoreboardAtEndDesc }}</p>
          </div>
          <div class="form-group col-md-4">
            <label>{{ T.contestNewFormPartialScore }}</label>
            <select class="form-control" v-model="partialScore">
              <option v-bind:value="true">
                {{ T.wordsYes }}
              </option>
              <option v-bind:value="false">
                {{ T.wordsNo }}
              </option>
            </select>
            <p class="help-block">{{ T.contestNewFormPartialScoreDesc }}</p>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormPointDecrementFactor }}</label>
            <input
              class="form-control"
              size="4"
              type="text"
              v-model="pointsDecayFactor"
            />
            <p class="help-block">
              {{ T.contestNewFormPointDecrementFactorDesc }}
            </p>
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.wordsLanguages }}</label
            ><br />
            <multiselect
              v-model="languages"
              v-bind:options="Object.keys(allLanguages)"
              v-bind:multiple="true"
              v-bind:placeholder="T.contestNewFormLanguages"
              v-bind:close-on-select="false"
              v-bind:allow-empty="false"
            >
            </multiselect>
            <p class="help-block">{{ T.contestNewFormLanguages }}</p>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormBasicInformationRequired }}</label>
            <div class="checkbox">
              <label
                ><input type="checkbox" v-model="needsBasicInformation" />{{
                  T.wordsEnable
                }}</label
              >
            </div>
            <p class="help-block">
              {{ T.contestNewFormBasicInformationRequiredDesc }}
            </p>
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormUserInformationRequired }}</label>
            <select class="form-control" v-model="requestsUserInformation">
              <option value="no">
                {{ T.wordsNo }}
              </option>
              <option value="optional">
                {{ T.wordsOptional }}
              </option>
              <option value="required">
                {{ T.wordsRequired }}
              </option>
            </select>
            <p class="help-block">
              {{ T.contestNewFormUserInformationRequiredDesc }}
            </p>
          </div>
        </div>
        <div class="form-group">
          <button class="btn btn-primary" type="submit">
            {{
              update
                ? T.contestNewFormUpdateContest
                : T.contestNewFormScheduleContest
            }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<style lang="scss">
@import '../../../../../../node_modules/vue-multiselect/dist/vue-multiselect.min.css';

.multiselect__tag {
  background: #678dd7;
}
</style>

<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator';
import { omegaup } from '../../omegaup';
import T from '../../lang';
import DateTimePicker from '../DateTimePicker.vue';
import Multiselect from 'vue-multiselect';

@Component({
  components: {
    'omegaup-datetimepicker': DateTimePicker,
    Multiselect,
  },
})
export default class NewForm extends Vue {
  @Prop() update!: boolean;
  @Prop() allLanguages!: string[];
  @Prop({ default: '' }) initialAlias!: string;
  @Prop({ default: '' }) initialDescription!: string;
  @Prop({ default: 'none' }) initialFeedback!: string;
  @Prop() initialFinishTime!: Date;
  @Prop({ default: false }) initialNeedsBasicInformation!: boolean;
  @Prop({ default: 0 }) initialPenalty!: number;
  @Prop({ default: 'none' }) initialPenaltyType!: string;
  @Prop({ default: 0.0 }) initialPointsDecayFactor!: number;
  @Prop({ default: 'no' }) initialRequestsUserInformation!: string;
  @Prop({ default: 100 }) initialScoreboard!: number;
  @Prop({ default: true }) initialShowScoreboardAfter!: boolean;
  @Prop({ default: true }) initialPartialScore!: boolean;
  @Prop() initialStartTime!: Date;
  @Prop() initialSubmissionsGap!: number;
  @Prop({ default: '' }) initialTitle!: string;
  @Prop({ default: null }) initialWindowLength!: null | number;

  T = T;
  alias = this.initialAlias;
  description = this.initialDescription;
  feedback = this.initialFeedback;
  finishTime = this.initialFinishTime;
  languages = Object.keys(this.allLanguages);
  needsBasicInformation = this.initialNeedsBasicInformation;
  penalty = this.initialPenalty;
  penaltyType = this.initialPenaltyType;
  pointsDecayFactor = this.initialPointsDecayFactor;
  requestsUserInformation = this.initialRequestsUserInformation;
  scoreboard = this.initialScoreboard;
  showScoreboardAfter = this.initialShowScoreboardAfter;
  partialScore = this.initialPartialScore;
  startTime = this.initialStartTime;
  submissionsGap = this.initialSubmissionsGap
    ? this.initialSubmissionsGap / 60
    : 1;
  title = this.initialTitle;
  windowLength = this.initialWindowLength;
  windowLengthEnabled = this.initialWindowLength !== null;
  titlePlaceHolder = '';

  @Watch('windowLengthEnabled')
  onPropertyChange(newValue: boolean): void {
    if (!newValue) {
      this.windowLength = null;
    }
  }

  fillOmi(): void {
    this.languages = Object.keys(this.allLanguages);
    this.titlePlaceHolder = T.contestNewFormTitlePlaceholderOmiStyle;
    this.windowLengthEnabled = false;
    this.windowLength = 0;
    this.scoreboard = 0;
    this.pointsDecayFactor = 0;
    this.submissionsGap = 1;
    this.feedback = 'detailed';
    this.penalty = 0;
    this.penaltyType = 'none';
    this.showScoreboardAfter = true;
  }

  fillPreIoi(): void {
    this.languages = Object.keys(this.allLanguages);
    this.titlePlaceHolder = T.contestNewFormTitlePlaceholderIoiStyle;
    this.windowLengthEnabled = true;
    this.windowLength = 180;
    this.scoreboard = 0;
    this.pointsDecayFactor = 0;
    this.submissionsGap = 0;
    this.feedback = 'detailed';
    this.penalty = 0;
    this.penaltyType = 'none';
    this.showScoreboardAfter = true;
  }

  fillConacup(): void {
    this.languages = Object.keys(this.allLanguages);
    this.titlePlaceHolder = T.contestNewFormTitlePlaceholderConacupStyle;
    this.windowLengthEnabled = false;
    this.windowLength = 0;
    this.scoreboard = 75;
    this.pointsDecayFactor = 0;
    this.submissionsGap = 1;
    this.feedback = 'detailed';
    this.penalty = 20;
    this.penaltyType = 'none';
    this.showScoreboardAfter = true;
  }

  fillIcpc(): void {
    const languagesKeys = Object.keys(this.allLanguages);
    this.languages = languagesKeys.filter(
      (lang) =>
        lang.includes('c11') ||
        lang.includes('cpp') ||
        lang.includes('py') ||
        lang.includes('java'),
    );
    this.titlePlaceHolder = T.contestNewFormTitlePlaceholderICPCStyle;
    this.windowLengthEnabled = false;
    this.windowLength = null;
    this.scoreboard = 80;
    this.pointsDecayFactor = 0;
    this.submissionsGap = 1;
    this.feedback = 'none';
    this.penalty = 20;
    this.penaltyType = 'contest_start';
    this.showScoreboardAfter = true;
    this.partialScore = false;
  }

  onSubmit() {
    if (this.update) {
      this.$emit('emit-update-contest', this);
      return;
    }
    const contest: omegaup.Contest = {
      alias: this.alias,
      title: this.title,
      description: this.description,
      start_time: this.startTime,
      finish_time: this.finishTime,
      window_length: !this.windowLengthEnabled ? null : this.windowLength,
      points_decay_factor: this.pointsDecayFactor,
      submissions_gap: (this.submissionsGap || 1) * 60,
      languages: this.languages,
      feedback: this.feedback,
      penalty: this.penalty,
      scoreboard: this.scoreboard,
      penalty_type: this.penaltyType,
      show_scoreboard_after: this.showScoreboardAfter,
      partial_score: this.partialScore,
      needs_basic_information: this.needsBasicInformation,
      requests_user_information: this.requestsUserInformation,
    };
    this.$emit('create-contest', contest);
  }
}
</script>
