---
title: Kafka - (1) Kafka란?
date: "2020-10-24T00:00"
template: "post"
draft: false
slug: "what-is-kafka"
category: "Kafka"
tags:
  - "Kafka"
  - "Apache"
description: "스트리밍 데이터를 사용하기 위해 만들어진 Pub-Sub 모델의 메시지 큐. 고성능, 고가용성의 메시지 애플리케이션이며 클러스터로 구성되어 분산환경에 특화되어 있다."
---

## Kafka란?
- 스트리밍 데이터를 사용하기 위해 만들어진 `pub-sub`(펍섭)모델의 메세지 큐
  *  `pub-sub`(펍섭)모델이란? : 중앙에 메시징 시스템 서버를 두고 메시지를 보내고(`Pub`lish) 받는(`Sub`scribe) 형태의 통신
- 클러스터로 구성되어 있으며 분산환경에 특화되어 있음
- 고성능, 고가용성 메시징 애플리케이션
  * 고가용성이란?: 지속적으로 운영할 수 있는 특성을 의미. 고장나서 중단되는 경우가 적다는 뜻으로 보면 됨

## Kafka의 특징
#### 프로듀서와 컨슈머의 분리
- 데이터를 보내는 역할과 받는 역할이 완벽하게 분리된 Pub/Sub 모델
- 만일 Pub/Sub 모델이 아니라 M개의 프로듀서와 N개의 컨슈머가 서로 강결합되어있는 구조라면 M*N 만큼의 관계를 맺어 복잡했을 것이다(유지보수도 매우 어렵게 된다). 하지만 카프카(_브로커_)가 중간에 껴서 중개를 해줌으로써 _프로듀서_ 와 _컨슈머_ 가 완전히 분리되면, M개의 프로듀서가 그저 카프카로 데이터를 제공하고 N개의 컨슈머는 그저 카프카에 있는 데이터를 가져오면 되므로 M+N만큼의 관계만 맺으면 된다.

#### 멀티 프로듀서, 멀티 컨슈머
- 한 종류의 데이터를 다양한 용도로 사용하고자 할 때 중복 데이터를 생산할 필요가 없음. 한 토픽에만 데이터를 쌓아놓으면 _그룹_ 이 서로 다른 여러 컨슈머에서 사용 가능하다.
- 마찬가지로, 여러 프로듀서에서 하나의 토픽에 데이터를 쌓을 수 있다.

#### 디스크에 메시지 저장
- 컨슈머의 처리가 늦어지거나 중단되더라도 카프카의 디스크에 데이터가 안전하게 보관되어 있어 손실없이 재개가 가능하다.
- 일반적인 메시징 시스템은 컨슈머가 메시지를 읽어가면 큐에서 바로 메시지를 삭제하지만, 카프카는 정해져있는 보관 주기 동안 디스크에 메시지를 저장하는 것이다.

#### 확장성
- 카프카 서비스의 중단없이 스케일아웃을 간단하게 할 수 있다.
- 장비 용량에 한계가 있으면 브로커를 추가해서 파티션수도 늘리면 된다.
- 컨슈머의 컨슘 속도가 느려서 _Lag_ 이 쌓인다면 컨슈머수와 파티션수를 함께 늘려주면 된다.

#### 높은 성능
- 분산시스템: 여러 대의 브로커로 분산처리
- OS 페이지캐시 사용: 파티션에 대한 파일IO를 메모리에서 처리
  - 메모리에 별도의 캐시를 구현하지 않고 OS의 페이지 캐시에 위임하고 OS가 알아서 서버의 유휴 메모리는 페이지 캐시로 사용하여, 앞으로 필요한 것으로 예상되는 메시지들을 미리 읽어들여 디스크 읽기 성능을 향상 시킨다.
  - 즉 영속성 차원에서 디스크에 데이터를 저장하긴 하지만, 높은 처리량을 위해 실제로는 OS 페이지캐시를 이용해 IO를 처리하는 경우가 대부분이라고 보여진다. 거의 인메모리 방식에 가까워 보인다.
  - Kafka 프로세스가 직접 캐시를 관리하지 않고 OS에 위임하므로, 프로세스 재시작 후 캐시를 워밍업할 필요가 없다는 장점이 있다.
- Zero Copy: 디스크 버퍼에서 네트워크 버퍼로 직접 데이터 복사
- 브로커의 역할이 Simple함: 메시지 필터, 재전송 등의 일은 브로커가 아니라  프로듀서, 컨슈머 단에서 함
- Batch처리: 프로듀서는 일정 크기만큼 메시지를 모아서 전송 가능하고, 컨슈머는 최소 크기만큼 메시지를 모아서 조회 가능


> [참고자료]  
> 고승범 외(2018), _카프카: 데이터 플랫폼의 최강자_, 책만.  
> https://level-muscle-c3a.notion.site/Kafka-665a8d1344c6496a8cd9770365708c17  

---

#### 다음 글
[(2) Producer, Broker, Consumer](/posts/kafka-producer-broker-consumer)