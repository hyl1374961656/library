//数据持久化
      import Vue from 'vue'
      import Vuex from 'vuex'
      import { post } from '@/utils/request'
      import createPersistedState from "vuex-persistedstate";

      Vue.use(Vuex)

      const moduleA = {
        namespaced: true,

        state: {
          name: 'moduleA',
          age: '1'
        },

        mutations: {
          increment () {
            console.log(this.state)
            console.log('moduleA')
          },
        },

        actions: {
          onIncrement (context) {
            context.commit('increment')
          }
        }
      }

      const store = new Vuex.Store({

          state: {
            count: 0,
            name: '郝云龙',
            age: 20,
            sex: '男',
            data: [],
          },

          mutations: {
            increment (state) {
              state.count++
            },
            setData (state, action) {
              state.data = action
            }
          },

          actions: {
            onIncrement (context) {
              post('/Home/Apis/listWithPage')
                .then(res => {
                context.commit('setData', res)
              })
          }
        },

        modules: {
          moduleA
        },

        plugins: [createPersistedState({
          key: 'vuex-data',
          reducer(option) {
            console.log(option)
            return {
              name: option.moduleA.name
            }
          }
        })]

      })
      store.dispatch('moduleA/onIncrement')

      export default store
