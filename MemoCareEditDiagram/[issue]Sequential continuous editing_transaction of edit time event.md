```mermaid
sequenceDiagram
    Client->>+Server: [POST] edit time event
    Server-->>-Client: [POST] response
    Client->>+Server: [GET] time event List
    Server-->>-Client: [GET] response

```

```mermaid
sequenceDiagram
    par [Time event 편집 Post, get 세트]
        Client->>Server: [POST1] edit time event
        Client->>+Server: [GET] Post1에 해당하는 time event List
    end
    par [Time event 편집 Post, get 세트]
        Client->>Server: [POST2] edit time event
        Client->>+Server: [GET] Post2에 해당하는 time event List
    end
    Server-->>-Client: [GET] Post2에 해당하는 time event List Response
    Server-->>-Client: [GET] Post1에 해당하는 time event List Response

```

 <!-- time event(af) 제외 편집transaction -->

```mermaid
sequenceDiagram
    Client->>+Server: [POST] edit time event
    Server-->>-Client: [POST] edit time event
    Client->>+Server: [GET] time event List
    Server-->>-Client: [GET] time event List

```

 <!-- af time event 편집 transaction -->

```mermaid
sequenceDiagram
    autonumber

    note over Client: time event 편집 transaction
    Client->>+Server: [POST] edit time event
    Server-->>-Client: [POST] edit time event
    Client->>+Server: [GET] time event List
    Server-->>-Client: [GET] time event List

    par parallel api 요청
    Client->>+Server: [GET] beat event
    note over Client: beat 편집
    Client->>+Server: [fetch] beat event
    end
    Server-->>-Client: [fetch] beat event
    Server-->>-Client: [GET] beat event

    note left of Client: [🚨issue] time event 편집 transaction 마지막이 beat 편집 res보다 늦게 왔을때 stale한 정보를 보여주게 된다.
    %% note over Client, Server: time event 편집 transaction 마지막이 beat 편집 res보다 늦게 왔을때 stale한 정보를 보여주게 된다.

```

```mermaid
%% 개선
sequenceDiagram
    autonumber

    note over Client: time event(af) 편집 transaction
    Client->>+Server: [POST] edit time event
    Server-->>-Client: [POST] edit time event
    Client->>+Server: [GET] time event List
    Server-->>-Client: [GET] time event List

    Client->>+Server: [GET] beat event
    Server-->>-Client: [GET] beat event
    note left of Client: [🚧작업2-1]: prevent setting redux state(event marker에 반영x)
    note left of Client: [🚧작업1]: af time event transaction중 마지막 task(post process) api([get] beat event) serialize 적용

    note over Client: beat 편집
    Client->>+Server: [fetch] beat event
    Server-->>-Client: [fetch] beat event

    note left of Client: [🚧작업2-2]: 작업 2-1에서 redux state업데이트 하지 방지했던 api 다시 조회
    Client->>+Server: [GET] beat event
    Server-->>-Client: [GET] beat event

    note left of Client: [작업2-1, 작업2-2]는 af편집시 post Process로 동작하는 task이후에 바로 비트 편집 task이 대기 하고있다면 동작합니다.

```

<!-- 순서가 보장되지 않는 api 1 -->

```mermaid
sequenceDiagram
    Client->>Server: [GET] edit time event 1 - req
    activate Server
    Client->>Server: [GET] edit time event 2 - req
    activate Server
    Server-->>Client: [GET] edit time event 2
    deactivate Server
    Server-->>Client: [GET] edit time event 1
    deactivate Server
```

```mermaid
sequenceDiagram
    Client->>+Server: [GET] edit time event 1 - req
    Client->>+Server: [GET] edit time event 2 - req
    Server-->>-Client: [GET] edit time event 2
    Server-->>-Client: [GET] edit time event 1
```

<!-- trade id가 필요한 api 1 -->

```mermaid
sequenceDiagram
    Client->>+Server: [POST] edit time event
    Server-->>-Client: [GET] time event List
    Client->>+Server: [GET] time event List
    Server-->>-Client: [POST] edit time event
```

```mermaid
sequenceDiagram
    Alice ->> John: First question
    activate(q1) John
    Alice ->> John: Second question
    activate(q2) John
    John -->> Alice: First answer
    deactivate(q1) John
    John -->> Alice: Second answer
    deactivate(q2) John
```

```mermaid

sequenceDiagram
Activate C1
A->>C: apple
Activate C2
B->>C: orange
C-->>A: banana
Deactivate C1
C-->>B: pear
Deactivate C2

```
