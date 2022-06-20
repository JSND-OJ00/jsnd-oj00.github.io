# 220620

# Ethereum

[https://www.preethikasireddy.com/post/how-does-ethereum-work-anyway](https://www.preethikasireddy.com/post/how-does-ethereum-work-anyway)

## Accounts

이더리움의 글로발 shared-states는 서로 상호작용 가능한 다수의 작은 객체  ‘accounts’로 구성되어 잇습니다. 각각의 accout는 이와 연관된 statedhk  20-byte의 address를 지닙니다. 

account에는 두 가지 종류가 있습니다. 

- Extermally owned accounts - 개인키로 제어, code 없음.
- Contract accounts - contract code로 제어. contract code를 지님.

Externally owned accounts vs. contract accounts

EOA는 개인키로 트랜잭션에 서명하여 다른 EOA 혹은 CA에 메시지를 보낼 수 있습니다. 두 EOA 사이의 메시지는 단순한 value의 전송입니다. 반면 EOA에서 CA로 메시지를 보내게 되면 다양한 동작을 위한 contract code가 실행됩니다(token의 전송, 내부 storage에 작성, 새로운 token 발행, 계산, 새로운 contract 생성 등).

EOA와 달리 CA는 스스로 트랜잭션을 발생시킬 수 없습니다. 대신, EOA 혹은 다른 CA로 부터 받은 트랜잭션에 대한 response로서 트랜잭션을 발생시킬 수 있습니다. 

따라서 이더리움 블록체인 상에서의 모든 행동은 EOA에서 발생한 트랜잭션으로 인한 것입니다.

## Account state

account의 state는 account의 종류와 상관없이 네 가지로 구성되어 있습니다.

- nonce: EOA의 경우 트랜잭션의 번호, CA의 경우 contract의 번호.
- balance: 해당 address가 지니는 wei의 갯수.
- storageRoot: Merkle Patricia tree의 루트 노드의 해시.
- codeHash: 해당 계정의 EVM(Ethereum Virtual Machine) code의 해시.

## World state

이더리움의 global state는 account address와 account state의 mapping으로 구성되어 있다. 이 mapping은 Merkle Patricia tree라는 데이터 구조로 저장된다.

Merkle tree는 다음과 같은 노드로 이루어진 이진 트리의 일종이다.

- 바닥에 있는 lead 노드는 기본적인 데이터를 지닌다.
- 중간 node는 두 자식 노드의 해시를 지닌다.
- 단 하나 뿐인 루트 노드 역시 두 자식 노드의 해시를 지니먀 트리의 정점에 위치한다.

…계속
