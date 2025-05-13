## Объяснение связей
```
┌────────────────────┐
│  [sipnet-auth]     │◄────────────────────────────┐
│ type = auth        │                             │
└────────────────────┘                             │
                                                   │
┌────────────────────┐        ┌──────────────────┐ │
│  [sipnet-aor]       │◄───────┤ [sipnetendpoint] │ │
│ type = aor          │        │ type = endpoint  │ │
└────────────────────┘        │ aors = sipnet-aor │ │
                               │ outbound_auth ────┘
                               │ (auth для вызовов)
                               │ context = my
                               └────────▲──────────┐
                                        │          │
                                        │          ▼
                          ┌─────────────────────────────┐
                          │      [sipnet-registration]   │
                          │ type = registration          │
                          │ outbound_auth = sipnet-auth  │
                          │ client_uri / server_uri      │
                          └─────────────────────────────┘

┌────────────────────┐
│   [idsipnet]        │
│ type = identify     │
│ match = 212.53.40.40│
│ endpoint = sipnetendpoint ─────────────────────────► [sipnetendpoint]
└────────────────────┘
```

Объяснение связей:

    [sipnetendpoint]:

        Использует aors = sipnet-aor — адрес регистрации.

        Использует outbound_auth = sipnet-auth — для авторизации исходящих вызовов.

        Связан с [idsipnet], который указывает, что пакеты от IP 212.53.40.40 (или домена sipnet.ru) соответствуют этому endpoint.

    [sipnet-registration]:

        Указывает outbound_auth = sipnet-auth — нужна для авторизации на SIP-сервере.

        client_uri и server_uri указывают, кто и куда регистрируется.

    [idsipnet]:

        Используется для идентификации входящих SIP-соединений по IP-адресу.

        Привязывает IP/домен к endpoint’у sipnetendpoint.


