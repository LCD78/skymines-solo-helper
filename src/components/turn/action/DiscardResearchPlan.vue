<template>
  <div class="actionBox col" data-bs-toggle="modal" data-bs-target="#modalDiscardResearchPlanHelp">
    <template v-if="isSpecialPlan">
      <span v-html="t('turnBot.action.discardResearchPlan.discardSpecial')"></span>
    </template>
    <template v-else>
      <span v-if="isFirstCol" v-html="t('turnBot.action.discardResearchPlan.discardFirstCol',{plan:plan})"></span>
      <span v-else v-html="t('turnBot.action.discardResearchPlan.discard',{plan:plan})"></span>
      <div class="selection">
        <AppIcon type="selection" name="research-plan-selection" class="matrix"/>
        <table>
          <tr>
            <td v-for="(col,indexCol) in action.researchPlanRow" :key="indexCol">
              <span v-if="col.includes(slot)">X</span>
            </td>
          </tr>
        </table>
      </div>
    </template>
  </div>

  <div class="modal" tabindex="-1" id="modalDiscardResearchPlanHelp">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable modal-lg">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="staticBackdropLabel">{{t('turnBot.action.discardResearchPlan.help.title')}}</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" :aria-label="t('action.close')"></button>
        </div>
        <div class="modal-body">
          <p v-html="t('turnBot.action.discardResearchPlan.help.instruction')"></p>
          <p v-html="t('turnBot.action.discardResearchPlan.help.specialResearchPlan')"></p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">{{t('action.close')}}</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { CardAction } from '@/services/Card'
import ResearchPlanSelection from '@/services/enum/ResearchPlanSelection'
import Slot from '@/services/enum/Slot'
import { defineComponent, PropType } from 'vue'
import { useI18n } from 'vue-i18n'
import AppIcon from '../../structure/AppIcon.vue'

export default defineComponent({
  name: 'DiscardResearchPlan',
  setup() {
    const { t } = useI18n()
    return { t }
  },
  components: {
    AppIcon
  },
  props: {
    action: {
      type: Object as PropType<CardAction>,
      required: true
    },
    cardSlot: {
      type: String,
      required: true
    }
  },
  computed: {
    slot() : Slot {
      return this.cardSlot as Slot
    },
    plan() : string {
      switch (this.action.researchPlanSelection) {
        case ResearchPlanSelection.PLAN_A_MATCH_SLOT:
          return 'A'
        case ResearchPlanSelection.PLAN_B_MATCH_SLOT:
          return 'B'
        case ResearchPlanSelection.PLAN_C_MATCH_SLOT:
          return 'C'
        default:
          return ''
      }
    },
    isFirstCol() : boolean {
      const row = this.action.researchPlanRow ?? []
      return row.length > 0 && row[0].includes(this.slot)
    },
    isSpecialPlan() : boolean {
      return this.action.researchPlanSelection == ResearchPlanSelection.SPECIAL_RESEARCH
    }
  }
})
</script>

<style lang="scss" scoped>
.selection {
  position: relative;
  margin-left: 1rem;
  width: 8rem;
  .matrix {
    width: 8rem;
  }
  table {
    position: absolute;
    left: 0rem;
    top: -0.1rem;
    z-index: 100;
    width: 8rem;
    height: 2.5rem;
    td {
      text-align: center;
      font-size: 1.1rem;
      font-weight: bold;
      width: calc(100%/4);
      height: 100%;
    } 
  }
}
</style>
