<template>
  <div>
    <Panel>
      <div slot="title">{{$t('m.Problems_List')}}</div>
      <Table v-if="contestRuleType == 'ACM' || OIContestRealTimePermission"
             :columns="ACMTableColumns"
             :data="problems"
             @on-row-click="goContestProblem"
             no-data-text="No Problems"></Table>
      <Table v-else
             :data="problems"
             :columns="OITableColumns"
             @on-row-click="goContestProblem"
             no-data-text="No Problems"></Table>
    </Panel>
  </div>
</template>

<script>
  import {mapState, mapGetters} from 'vuex'
  import {ProblemMixin} from '@oj/components/mixins'
  import api from '@oj/api'
  import { USER_TYPE } from '@/utils/constants'
  export default {
    name: 'ContestProblemList',
    mixins: [ProblemMixin],
    data () {
      return {
        ACMTableColumns: [
          {
            title: '#',
            key: '_id',
            sortType: 'asc',
            width: 150
          },
          {
            title: 'Title',
            key: 'title'
          },
          {
            title: 'Total',
            key: 'submission_number'
          },
          {
            title: 'AC Rate',
            render: (h, params) => {
              return h('span', this.getACRate(params.row.accepted_number, params.row.submission_number))
            }
          }
        ],
        OITableColumns: [
          {
            title: '#',
            key: '_id',
            width: 150
          },
          {
            title: 'Title',
            key: 'title'
          }
        ],
        ContestProblemEnter_column: false,
        ContestProblemRejudge_column: false
      }
    },
    mounted () {
      this.getContestProblems()
    },
    methods: {
      getContestProblems () {
        this.$store.dispatch('getContestProblems').then(res => {
          if (this.isAuthenticated) {
            if (this.contestRuleType === 'ACM') {
              this.addStatusColumn(this.ACMTableColumns, res.data.data)
            } else if (this.OIContestRealTimePermission) {
              this.addStatusColumn(this.ACMTableColumns, res.data.data)
            }
          }
          this.adjustContestProblemRejudgeColumn()
        })
      },
      goContestProblem (row) {
        this.$router.push({
          name: 'contest-problem-details',
          params: {
            contestID: this.$route.params.contestID,
            problemID: row._id
          }
        })
      },
      adjustContestProblemRejudgeColumn () {
        if (this.ContestProblemRejudge_column || !this.ContestProblemRejudgeColumnVisible) {
          return
        }
        const RejudgeColumn = {
          title: 'Admin Option',
          fixed: 'right',
          align: 'center',
          width: 120,
          render: (h, params) => {
            return h('Button', {
              props: {
                type: 'error',
                size: 'small'
              },
              on: {
                click: (event) => {
                  event.stopPropagation()
                  this.$Modal.confirm({
                    width: 350,
                    loading: false,
                    title: 'Rejudge Comfirm',
                    cancelText: 'Cancel',
                    okText: 'Rejudge',
                    content: 'Really want to rejudge problem?',
                    onOk: () => {
                      this.loading = true
                      this.handleContestProblemRejudge(params.row._id, this.$route.params.contestID)
                      this.loading = false
                    }
                  })
                }
              }
            }, 'Rejudge')
          }
        }
        if (this.contestRuleType === 'ACM') {
          this.ACMTableColumns.push(RejudgeColumn)
        } else if (this.contestRuleType === 'OI') {
          this.OITableColumns.push(RejudgeColumn)
        }
        this.ContestProblemRejudge_column = true
      },
      handleContestProblemRejudge (problemID, contestID) {
        api.ContestProblemRejudge(problemID, contestID).then(() => {
          this.$success('Rejudge Successed')
        }, () => {
        })
      }
    },
    computed: {
      ...mapState({
        problems: state => state.contest.contestProblems
      }),
      ...mapGetters(['isAuthenticated', 'contestRuleType', 'OIContestRealTimePermission', 'user']),
      /**
       * @return {boolean}
       */
      ContestProblemRejudgeColumnVisible () {
        return this.$route.params.contestID && this.user.admin_type === USER_TYPE.SUPER_ADMIN
      }
    }
  }
</script>

<style scoped lang="less">
</style>
