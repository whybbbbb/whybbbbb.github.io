# Layer 2

## contents

1. Why Layer 2
2. Rollup Type
	1. Optimistic Rollup
	2. ZK Rollup
3. EVM vs ZK-EVM
4. Layer 2 SDK tools..
5. EIP-4844와 Layer 2

## Why Layer 2

이더리움(Layer 1)은 참여 노드의 숫자와 그에 비례하여 투입된 자원(H/W power) 비해 너무 낮은 throughput과 latency를 가진다. 이런 확장성에 관한 문제를 해결하기 위한 방법으로 단순하게 생각해보면 제 3자를 이용하는 방법과 블록에 등록되는 tx의 수를 대폭 늘리는 것이 있지만, 제 3자를 이용할 경우 그 3자에 대한 신뢰(탈중앙화 실패)문제가 발생하게 되고, tx의 수를 늘린다는 것은 오히려 자원 투입량이 늘어나는 문제를 발생시킬 것이다.

Offchain(Layer 2) rollup은 확장성 문제를 해결하기 위해 나온 새로운 방법이다. Rollup은 offchain에서 신뢰할 수 있는 방법을 통해 Tx를 처리한 후 이를 배치 처리하여 single-transaction으로 이더리움 체인에 제출하는 기술이다. 이를 통해 블록 사이즈를 늘리지 않더라도 tx를 처리할 수 있게 되고, 이더리움 노드(geth, besu 등) 운영에 사용되는 자원이 offchain 노드를 위해 사용된다면 이더리움 노드의 운영을 위해 투입되는 H/W power의 총량 또한 줄어들 수 있게 되는 것이다.



## Rollup Type

Offchain마다 다양한 방식의 rollup이 사용되지만, 이 문서에서는 optimistic rollup과 zk rollup에 대해서만 다룬다.(plasma 등 타 방식은 현재 유효한 결과물을 내놓지 못하고 있음.)

## 1. Optimistic Rollup

Optimistic Rollup은 fault tx가 발생하지 않을 것이라는 낙관적인 기대를 바탕으로 Off-chain이 tx를 처리한 후 그 결과를 single-transaction으로 만들어 이더리움 메인넷에 기록하는 방법이다. 이를 통해 블록의 보안, 완결, 신뢰성은 이더리움 체인에 위임하고, Optimistic rollup 기반의 offchain은 tx수행 등의 체인의 상태 변경에 대해 집중한다.

### Optimism

Optimism은 Optimistic rollup을 사용하는 대표적인 layer2 offchain중 하나이다. 시퀀서(sequencer)라는 메인 노드가 아래 세가지 핵심 기능을 처리한다.

- layer1 - layer2 간의 자산 전송 tx 처리([L1StandardBridge](https://github.com/ethereum-optimism/optimism/blob/65ec61dde94ffa93342728d324fecf474d228e1f/packages/contracts-bedrock/contracts/L1/L1StandardBridge.sol) Contract)
- OP-Batcher : layer2의 tx 수집 및 실행(create merkle tree, state root)
- OP-Proposer : layer2의 tx 실행 결과를 layer1에 기록(merkle tree, state root)

시퀀서 노드가 fault tx를 수행하는 것에 대비하여 감시자(verifier) 노드가 따로 구성되어 시퀸서 노드가 layer1에 제출한 tx를 수행하며, merkle tree, state root와 layer2의 tx의 수행 내역이 정확히 일치하는지를 검증하며 문제가 발생할 시 challenge를 통해 rollback 절차를 수행한다.

**하지만 검증자의 딜레마를 해결하지 못한 문제점이 있다.**
1. 시스템이 정상적이면 문제가 발생하지 않음
2. 문제가 발생하지 않으면 검증자는 인센티브를 받을 수 없음
3. 인센티브를 받지 못하는 검증자는 검증을 포기
4. 검증자가 사라져 시스템이 붕괴

Layer1과 Layer2는 스마트컨트랙을 기반으로 상호 통신한다. ([l1-l2 contract](https://community.optimism.io/docs/protocol/protocol-2.0/#))

### Arbitrum

Arbitrum 역시 Optimistic rollup을 사용하는 layer2 offchain중 하나이다. Optimism과 유사한 시퀀서가 tx처리, layer간 연동, batch 등록을 담당한다.

과거 Arbitrum은 AVM이라는 VM을 사용했었는데, 이로 인해 EVM 100% 호환을 지원하지 못하였다. 그래서 최근 Arbitrum Nitro 업데이트를 진행하여 EVM(GETH)의 상위 레이어에 Arb OS라는 레이어(L1-L2 bridging 등 layer간 연동을 위한 기능)를 추가한 노드를 만들어 EVM 호환을 지원한다.

## 2. ZK Rollup

## 특이할만한 차이점

### 1. Validation 방식

### 2. 

# Optimism

## Optimistic Rollup

