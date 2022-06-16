# 220616

# Ethereum

[https://www.preethikasireddy.com/post/how-does-ethereum-work-anyway](https://www.preethikasireddy.com/post/how-does-ethereum-work-anyway)

번역, 귀찮으면 중간중간 생략

## ****The Ethereum blockchain paradigm explained****

이더리움 블록체인은 근본적으로 트랜잭션에 기반한 state-machine이다. 컴퓨터 공학에서 state-machine이란, input을 읽어들이고 input을 바탕으로 새로운 state로 변환하는 것을 뜻한다.

어더리우 state-machine은 ‘genesis state’에서 시작된다. 네트워크에서 어떠한 트랜잭션이 발생하기 전에는 빈 슬레이트와 흡사하다. 트랜잭션이 실행되면 이 genesis state는 final state로 변하게 되며, 어떠한 시점에서든 final state가 이더리움의 currenet state가 된다.

이더리움의 state는 아주 많은 트랜잭션을 지니며, 트랜잭션은 블록이라는 단위로 묶인다. 블록은 다수의 state를 지니며 각각의 블록은 이전의 블록과 연결되어 있다.

특정 state에서 다음 state로 이행하기 위해 트랜잭션은 반드시 검증되어야 한다. 트랜잭션이 검증되었다고 인식되기 위하여 마이닝이라 불리는 검증 절차를 반드시 거쳐야 한다. 그룹에 속한 노드(컴퓨터 등)들은 스스로의 컴퓨터 자원을 소모하여 건증된 트랜잭션으로 구성된 블록을 만든다.

네트워크에 속한 모든 노드는 스스로를 마이너라고 선언하여 검증된 블록의 생성을 시도할 수 있다. 지금도 세계각지의 수많은 마이너들이 블록을 만들고 검증하기 위해 시도 하고 있다. 이더리움의 검증 절차는 작업증명(proof of work)이라 불리며, 새로운 블록의 검증에 성공한 마이너는 특정한 가치를 보상으로 받는데, 이더리움 네트워크의 경이 이더(ether)가 이 보상에 해당한다.

하지만 만약 누군가가 연결된 블록에서 갈라져 나온 새로운 체인을 만들기 위한 시도(fork)가 행해진다면 어떻게 될까? 이더리움 블록체인은 하나의 state만 지녀야 하므로 네트위크는 봉괴될 것이다. 하나의 체인에서 갈라져 나온 복수의 체인이 각각의 state를 지닌다면 어느 것이 가장 옳은 것인지 확인할 방법이 없다. 이러한 사태를 방지하기 위해 이더리움은 ‘GHOST protocol’을 사용한다.

## “GHOST” = “Greedy Heaviest Observed Subtree”

GHOST protocol에 따라 우리는 가장 많은 연산이 행해진 path를 선택해야 한다. 이 path를 정하기 위해 가장 최근 블록(leaf block)의 블록 넘버가 사용되는데, 이는 genesis block을 제외한 현재 path 상에 존재하는 블록의 총 수를 뜻한다. 블록 넘버가 높다는 것은 path가 길다는 것이며, path가 길다는 것은 현재의 블록에 도단하기 위해 더 많은 컴퓨터 자원이 소모되었다는 것을 뜻한다.

내일은 account와 state에 대하여 다루어 보겠다.
