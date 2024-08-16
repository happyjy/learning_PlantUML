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
    Server-->>-Client: [GET] Post1에 해당하는 time event List Response
    Server-->>-Client: [GET] Post2에 해당하는 time event List Response

```

    deactivate Client
    <!-- client-->>+server: time event 편집 api
    server<<--client: time event 편집 api -->

    Note over client: A typical interaction<br/>But now in two lines

Alice->>+John: Hello John, how are you?
John-->>-Alice: Great!

```mermaid

sequenceDiagram
    Alice ->> John: First question
    activate John: q1
    Alice ->> John: Second question
    activate John: q2
    John -->> Alice: First answer
    deactivate John: q1
    John -->> Alice: Second answer
    deactivate John: q2

```
