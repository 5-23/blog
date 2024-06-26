---
title: 싱글톤 패턴
published: 2024-06-10
description: 싱글톤 디자인패턴을 알아보자
image: thumb.jpg
tags: [design pattern, singleton, 싱글톤, 디자인 패턴]
category: cs
draft: false
---
# 디자인패턴?
코드의 가독성을 높이기위해 만들어진 형식을 __디자인 패턴__ 이라 합니다.
`싱글톤`, `템플릿 메소드 패턴`, `상태 패턴`, ... 등등이 있고 오늘은 `싱글톤`패턴에 대해 알아옵시다.

## 싱글톤 패턴(Singleton Pattern)
싱글톤 패턴은 클래스가 객채 인스턴스를 하나만 가지고 있는패턴입니다.
```rs
struct 클레스 {
    인스턴스: ...
}
```

## 이걸 어디써?
싱글톤 패턴은 db와 관련된 객채를 만들때 많이 사용됩니다.<br/>
mongodb를 사용한다고 예를 들어본다면 아레와 같은 코드가 나오게 됩니다.

```rs
struct userDB {
    collection: Collection<UserData>
}
impl userDB {
    pub async fn connect() -> Result<Self, String> {
        dotenv::dotenv().ok();
        let uri = std::env::var("DBURI").unwrap();
        let client = mongodb::Client::with_uri_str(uri).await;
        if client.is_err() {
            return Err("Failed to connect to the database".to_string());
        }
        let collection = client
            .unwrap()
            .database("realSusDbNsme")
            .collection::<UserData>("users");
        Ok(Self { collection })
    }
}
```


## 단점
싱글톤 패턴은 TDD(Test Driven Development)에서 단위테스트를 할때 단위가 서로 독립적이여야하며,<br>
하지만 싱글톤 페턴은 하나의 인스턴스를 기반으로 구현하는 페턴이므로 테스트마가 각각의 독립적인 인스턴스를 만들기가 어렵습니다
