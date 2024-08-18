## 편집 이벤트 종류 두가지

- beat event
- time event

## 설명 방법

- 첫번째. command를 호출하는 흐름을 설명
- 두번째. command에서 어떤 과정을 통해 optimistic update를 호출하는지 설명

## beat event 편집시 command 호출 순서

- testResultduck.js > POST_BEATS_REQUESTED, PATCH_BEATS_REQUESTED, DELETE_BEATS_REQUESTED action의 saga function에서 부터 시작
- 위 saga function에서 "optimisticBeatEventUpdateFn" 호출

* optimisticBeatEventUpdateFn 역할
  - beat 편집 방법 4가지에 대한 command 객체를 strategy pattern으로 요청에 따라 알맞은 command execute
  - PostBeatCommand, PatchBeatByWaveformIndexListCommand, PatchBeatByRangeListCommand, DeleteBeatCommand

## time event 편집시 command 호출 순서

- testResultduck.js > POST_TIME_EVENT_REQUESTED action의 saga function에서 부터 시작
- fn\*| \_postTimeEvent

  - avBlockEvent 여부 판단
  - Step1. optimisticEventDataUpdate
    - fn| preProcessTimeEventEdit
  - Step2. Call api; edit time event Type
    - yield put| enqueueRequest
  - step3. Call api; fetching TimeEventList
    - enqueueRequest 성공 처리 과정인 succeedCallback 안에서 수행
  - step4. postProcess; edit event validation
    - step3. api 성공 이후 수행 (POST_PROCESS_EDITED_TIME_EVENT)
    - POST_PROCESS_EDITED_TIME_EVENT 과정은 taskqueue의 scheduling의 첫번째와 관련 있음

# scheduling algorithm 3가지

1. FETCH_BEAT_EVENT case

- scheduling 목적
  - FETCH_BEAT_EVENT 수행 될 때 take queue에 POST_PROCESS_EDITED_TIME_EVENT 있을 경우
  - 현재 수행 되는 fetching beat를 통해 받은 beat를 redux state에 업데이트 시키지 않음
- condition
  - enqueueTaskType가 FETCH_BEAT_EVENT

2. POST_TIME_EVENT case

- scheduling 목적
  - getTimeEvent를 최종적으로 한번만 해주기 위한 과정.
- condition
  - enqueueTaskType가 POST_TIME_EVENT 또는 GET_TIME_EVENTS_LIST 이면서
  - processingTaskType이 GET_TIME_EVENT가 아니거나,
  - processingTaskType이 GET_TIME_EVENT이면서 eventType이 다를 경우

3.  GET_EVENT_DETAIL case

- scheduling 목적
  - GET_EVENT_DETAIL을 최종적으로 한번만 해주기 위한 과정.
- condition
  - task queue에 추가되는 task가 GET_EVENT_DETAIL이고
  - task queue가 진행중인 task가 GET_EVENT_DETAIL이 아닐 경우
