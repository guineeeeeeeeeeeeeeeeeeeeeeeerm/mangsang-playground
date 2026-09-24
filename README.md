# mangsang-playground

[mangsang](https://github.com/guinjaaaaaaaaaaaaaaaaaaaaakeop/mangsang)이 여러 상황에서 어떻게 쓰이는지 하나씩 해 보는 곳.
시나리오 하나가 디렉터리 하나이고, 각 디렉터리가 mangsang의 target이다.

| 시나리오 | 상황 | 보기 |
|---|---|---|
| [rock-paper-scissors](rock-paper-scissors/) | 설명문 하나뿐인 프로젝트. 글의 절이 개념을 말하고, 개념이 질문에 답한다. 코드도 테스트도 없다 | [MODEL.md](rock-paper-scissors/MODEL.md) |

## 각 시나리오에 있는 것

- `README.md` — 사람이 읽는 글. 절(`##`)마다 mangsang의 anchor가 된다.
- `mangsang/` — mangsang이 관리하는 지식: 개념(`concepts/`), 관계(`relations/`), 원문(`sources/`), 질문(`cq/`), 서명할 수 있는 사람(`people.json`).
- `MODEL.md` — `mangsang report --out MODEL.md`가 만든 페이지. 개념과 그것을 말하는 절, 질문과 그 상태, 원문, 그리고 Mermaid 그래프.
  mangsang은 이 파일을 "derived, not committed"로 다루지만, 이 저장소의 목적이 그 그림을 GitHub에서 보는 것이라 예외로 커밋한다.
  손으로 고치지 않고, 모델이 바뀌면 다시 만든다.

## 어떻게 만들었나

```
M=<mangsang plugin root>/mangsang.py
cd rock-paper-scissors
python3 $M register README.md
python3 $M source add rps-01 --file - --speaker <who> --locator "..."     # 요청, 제안, 승낙 — 있는 그대로
python3 $M concept add hand --means "..." --by <who> --approved-in source:rps-03
python3 $M cq add what-beats-what --text "..." --verify '{"kind": "answered-by", "concepts": ["beats"]}' --by <who> --approved-in source:rps-03
python3 $M confirm proposals.json --delegated "..."                      # 절 -> 개념, 원문 -> 개념 관계, 인용과 함께
python3 $M check && python3 $M cq && python3 $M impact                   # 모두 초록
python3 $M report --out MODEL.md
python3 $M report --check MODEL.md                                       # 커밋한 페이지가 지금 기록과 같은가
```

mangsang 1.6.0. `MODEL.md`가 기록보다 뒤처졌는지는 `python3 $M report --check MODEL.md`가 말한다(뒤처졌으면 exit 1).
