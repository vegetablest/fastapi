# Lifespan 이벤트

애플리케이션 **시작 전**에 실행되어야 하는 로직(코드)을 정의할 수 있습니다. 이는 이 코드가 **한 번**만 실행되며, **애플리케이션이 요청을 받기 시작하기 전**에 실행된다는 의미입니다.

마찬가지로, 애플리케이션이 **종료될 때** 실행되어야 하는 로직(코드)을 정의할 수 있습니다. 이 경우, 이 코드는 **한 번**만 실행되며, **여러 요청을 처리한 후**에 실행됩니다.

이 코드가 애플리케이션이 **요청을 받기 시작하기 전에** 실행되고, 요청 처리가 끝난 후 **종료 직전에** 실행되기 때문에 전체 애플리케이션의 **수명(Lifespan)**을 다룹니다. (잠시 후 "수명"이라는 단어가 중요해집니다 😉)

이 방법은 전체 애플리케이션에서 사용해야 하는 **자원**을 설정하거나 요청 간에 **공유되는** 자원을 설정하고, 또는 그 후에 **정리**하는 데 매우 유용할 수 있습니다. 예를 들어, 데이터베이스 연결 풀 또는 공유되는 머신러닝 모델을 로드하는 경우입니다.


## 사용 사례

먼저 **사용 사례**를 예로 들어보고, 이를 어떻게 해결할 수 있는지 살펴보겠습니다.

우리가 요청을 처리하기 위해 사용하고 싶은 **머신러닝 모델**이 있다고 상상해 봅시다. 🤖

이 모델들은 요청 간에 공유되므로, 요청마다 모델이 하나씩 있는 것이 아니라, 여러 요청에서 동일한 모델을 사용합니다.

모델을 로드하는 데 **상당한 시간이 걸린다고 상상해 봅시다**, 왜냐하면 모델이 **디스크에서 많은 데이터를 읽어야** 하기 때문입니다. 따라서 모든 요청에 대해 모델을 매번 로드하고 싶지 않습니다.

모듈/파일의 최상위에서 모델을 로드할 수도 있지만, 그러면 **모델을 로드하는데** 시간이 걸리기 때문에, 단순한 자동화된 테스트를 실행할 때도 모델이 로드될 때까지 기다려야 해서 **테스트 속도가 느려집니다**.

이 문제를 해결하려고 하는 것입니다. 요청을 처리하기 전에 모델을 로드하되, 애플리케이션이 요청을 받기 시작하기 직전에만 로드하고, 코드가 로드되는 동안은 로드하지 않도록 하겠습니다.

## Lifespan

`FastAPI` 애플리케이션의 `lifespan` 매개변수와 "컨텍스트 매니저"를 사용하여 *시작*과 *종료* 로직을 정의할 수 있습니다. (컨텍스트 매니저가 무엇인지 잠시 후에 설명드리겠습니다.)

예제를 통해 시작하고, 그 후에 자세히 살펴보겠습니다.

우리는 `yield`를 사용하여 비동기 함수 `lifespan()`을 다음과 같이 생성합니다:

{* ../../docs_src/events/tutorial003.py hl[16,19] *}

여기서 우리는 모델을 로드하는 비싼 *시작* 작업을 시뮬레이션하고 있습니다. `yield` 앞에서 (가짜) 모델 함수를 머신러닝 모델이 담긴 딕셔너리에 넣습니다. 이 코드는 **애플리케이션이 요청을 받기 시작하기 전**, *시작* 동안에 실행됩니다.

그리고 `yield` 직후에는 모델을 언로드합니다. 이 코드는 **애플리케이션이 요청 처리 완료 후**, *종료* 직전에 실행됩니다. 예를 들어, 메모리나 GPU와 같은 자원을 해제하는 작업을 할 수 있습니다.

/// tip | 팁

`shutdown`은 애플리케이션을 **종료**할 때 발생합니다.

새로운 버전을 시작해야 하거나, 그냥 실행을 멈추고 싶을 수도 있습니다. 🤷

///

### Lifespan 함수

먼저 주목할 점은, `yield`를 사용하여 비동기 함수(async function)를 정의하고 있다는 것입니다. 이는 `yield`를 사용한 의존성과 매우 유사합니다.

{* ../../docs_src/events/tutorial003.py hl[14:19] *}

함수의 첫 번째 부분, 즉 `yield` 이전의 코드는 애플리케이션이 시작되기 **전에** 실행됩니다.

그리고 `yield` 이후의 부분은 애플리케이션이 완료된 후 **나중에** 실행됩니다.

### 비동기 컨텍스트 매니저

함수를 확인해보면, `@asynccontextmanager`로 장식되어 있습니다.

이것은 함수를 "**비동기 컨텍스트 매니저**"라고 불리는 것으로 변환시킵니다.

{* ../../docs_src/events/tutorial003.py hl[1,13] *}

파이썬에서 **컨텍스트 매니저**는 `with` 문에서 사용할 수 있는 것입니다. 예를 들어, `open()`은 컨텍스트 매니저로 사용할 수 있습니다:

```Python
with open("file.txt") as file:
    file.read()
```
최근 버전의 파이썬에서는 **비동기 컨텍스트 매니저**도 있습니다. 이를 `async with`와 함께 사용합니다:

```Python
async with lifespan(app):
    await do_stuff()
```

컨텍스트 매니저나 위와 같은 비동기 컨텍스트 매니저를 만들면, `with` 블록에 들어가기 전에 `yield` 이전의 코드가 실행되고, `with` 블록을 벗어난 후에는 `yield` 이후의 코드가 실행됩니다.

위의 코드 예제에서는 직접 사용하지 않고, FastAPI에 전달하여 사용하도록 합니다.

`FastAPI` 애플리케이션의 `lifespan` 매개변수는 **비동기 컨텍스트 매니저**를 받기 때문에, 새로운 `lifespan` 비동기 컨텍스트 매니저를 FastAPI에 전달할 수 있습니다.

{* ../../docs_src/events/tutorial003.py hl[22] *}

## 대체 이벤트 (사용 중단)

/// warning | 경고

*시작*과 *종료*를 처리하는 권장 방법은 위에서 설명한 대로 `FastAPI` 애플리케이션의 `lifespan` 매개변수를 사용하는 것입니다. `lifespan` 매개변수를 제공하면 `startup`과 `shutdown` 이벤트 핸들러는 더 이상 호출되지 않습니다. `lifespan`을 사용할지, 모든 이벤트를 사용할지 선택해야 하며 둘 다 사용할 수는 없습니다.

이 부분은 건너뛰셔도 좋습니다.

///

*시작*과 *종료* 동안 실행될 이 로직을 정의하는 대체 방법이 있습니다.

애플리케이션이 시작되기 전에 또는 종료될 때 실행해야 하는 이벤트 핸들러(함수)를 정의할 수 있습니다.

이 함수들은 `async def` 또는 일반 `def`로 선언할 수 있습니다.

### `startup` 이벤트

애플리케이션이 시작되기 전에 실행되어야 하는 함수를 추가하려면, `"startup"` 이벤트로 선언합니다:

{* ../../docs_src/events/tutorial001.py hl[8] *}

이 경우, `startup` 이벤트 핸들러 함수는 "database"라는 항목(단지 `dict`)을 일부 값으로 초기화합니다.

여러 개의 이벤트 핸들러 함수를 추가할 수 있습니다.

애플리케이션은 모든 `startup` 이벤트 핸들러가 완료될 때까지 요청을 받기 시작하지 않습니다.

### `shutdown` 이벤트

애플리케이션이 종료될 때 실행되어야 하는 함수를 추가하려면, `"shutdown"` 이벤트로 선언합니다:

{* ../../docs_src/events/tutorial002.py hl[6] *}

여기서, `shutdown` 이벤트 핸들러 함수는 `"Application shutdown"`이라는 텍스트를 `log.txt` 파일에 기록합니다.

/// info | 정보

`open()` 함수에서 `mode="a"`는 "추가"를 의미하므로, 파일에 있는 기존 내용은 덮어쓰지 않고 새로운 줄이 추가됩니다.

///

/// tip | 팁

이 경우, 우리는 표준 파이썬 `open()` 함수를 사용하여 파일과 상호작용하고 있습니다.

따라서 I/O(입출력) 작업이 포함되어 있어 디스크에 기록되는 것을 "기다리는" 과정이 필요합니다.

하지만 `open()`은 `async`와 `await`를 사용하지 않습니다.

그래서 우리는 이벤트 핸들러 함수를 `async def` 대신 일반 `def`로 선언합니다.

///

### `startup`과 `shutdown`을 함께 사용

*시작*과 *종료* 로직이 연결될 가능성이 높습니다. 예를 들어, 무언가를 시작한 후 끝내거나, 자원을 획득한 후 해제하는 등의 작업을 할 수 있습니다.

이러한 작업을 별도의 함수로 처리하면 서로 로직이나 변수를 공유하지 않기 때문에 더 어려워집니다. 값들을 전역 변수에 저장하거나 비슷한 트릭을 사용해야 할 수 있습니다.

그렇기 때문에 위에서 설명한 대로 `lifespan`을 사용하는 것이 권장됩니다.

## 기술적 세부사항

호기심 많은 분들을 위한 기술적인 세부사항입니다. 🤓

ASGI 기술 사양에 따르면, 이는 <a href="https://asgi.readthedocs.io/en/latest/specs/lifespan.html" class="external-link" target="_blank">Lifespan Protocol</a>의 일부이며, `startup`과 `shutdown`이라는 이벤트를 정의합니다.

/// info | 정보

Starlette의 `lifespan` 핸들러에 대해 더 읽고 싶다면 <a href="https://www.starlette.io/lifespan/" class="external-link" target="_blank">Starlette의 Lifespan 문서</a>에서 확인할 수 있습니다.

이 문서에는 코드의 다른 영역에서 사용할 수 있는 lifespan 상태를 처리하는 방법도 포함되어 있습니다.

///

## 서브 애플리케이션

🚨 이 lifespan 이벤트(`startup`과 `shutdown`)는 메인 애플리케이션에 대해서만 실행되며, [서브 애플리케이션 - Mounts](sub-applications.md){.internal-link target=_blank}에는 실행되지 않음을 유의하세요.
