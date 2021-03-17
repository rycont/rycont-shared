## 쓰레드란 무엇인가?
- 이건 딱히 배포목적으로 만드는건 아니고 그냥 끄적이는건데.. 그냥 공유하려고요~

쓰레드 : 프로세스를 구성하는 일들.
파이썬은 기본적으로는 한번에 하나의 쓰레드밖에 처리하지 못하는 **싱글스레드** 언어에요. 그렇지만 `threading` 모듈을 사용하면 한번에 여러개의 일을 수행(멀티쓰레드)할 수 있음!
### 쓰레드 기본개념
```python
import threading
```
`threading` 모듈을 사용해서 여러개의 일을 한번에 할 때는 `쓰레드 생성 -> 실행`의 순서를 거쳐요.
1. 쓰레드 생성은 `threading.Thread` 클래스를 사용합니다. `target` 인자로 실행할 작업을 함수로 넘겨주고, 필요한 경우 `name` 으로 스레드 이름을 지정해주거나(디버깅용), `args`, `kargs` 인자로 함수 실행에 필요한 인수를 넘겨줘요. `daemon` 인자를 True로 할 경우에는, 메인 스레드가 죽을 때, 이 스레드도 함께 죽여요.  
2. 쓰레드 실행은 `Thread` 인스턴스의 `start` 메소드를 이용해요. 

쓰레드를 활용하여 "1초 쉬기 - `파이썬 뿌셔` 출력 - 1초 쉬기 - `파이썬 뿌셔` 출력"을 수행하는 코드를 아래와 같이 작성하였지만 제대로 작동하지 않았습니다.
(올바른 예제는 아니지만 메소드의 학습을 위해 가정한 상황입니다.)
```python
import threading
import time

def action():
	time.sleep(1)
	print("파이썬 뿌셔!!")
	
task1 = threading.Thread(target=action)
task2 = threading.Thread(target=action)

task1.start()
task2.start()
```
쓰레드는 기본적으로 실행이 끝날 때 까지 기다리지 않기 때문입니다. 위 코드에서는 "1초 뒤에 `파이썬뿌셔`를 출력"이라는 작업을 만들긴 했지만, 작업이 완료되기 전에 프로그램의 실행이 끝나버려서 출력은 미처 수행하지 못합니다. 물론 스레드는 남아있기 때문에 묵묵히 자기 할 일을 하고 있죠.
그러면 아래와 같이 작성한 코드는 어떨까요?
```python
import threading
import time

def action():
	time.sleep(1)
	print("파이썬 뿌셔!!")
	
task1 = threading.Thread(target=action)
task2 = threading.Thread(target=action)

task1.start()
task2.start()
time.sleep(2)
```
이 또한 예상대로 작동하지는 않습니다. 1초 기다린 후에 `파이썬 뿌셔`가 두번 출력되는걸 볼 수 있습니다. 위에서 말한듯이, 쓰레드의 실행이 끝날 때 까지 기다리지 않기 때문에, task1와 task2가 동시에 실행됩니다(사실 동시는 아닌데 그냥 그렇다고 치죠.. 제 컴퓨터에서는 task2가 0.01초 늦게 실행됨..). 그렇기에 실행 1초 후 동시에 출력하게됩니다.
처음에 제시한 출력대로 쓰레드를 사용하려면 `join` 메소드를 사용합니다. `join` 메소드는 **쓰레드가 종료될 때 까지 기다려주는 역할**을 합니다. 
```python
import threading
import time

def action():
	time.sleep(1)
	print("파이썬 뿌셔!!")

task1 = threading.Thread(target=action)
task2 = threading.Thread(target=action)

task1.start()
task1.join()
task2.start()
task2.join()
```
`task1.join()`에 의해서 `task1`이 끝날 때 까지 기다리다가 `task2`의 시작이 1초 미뤄지게 되고, `task2.join()`에 의해서 `task2`가 끝날 때 까지 기다리고 콘솔에는 두 값 모두 출력됩니다.
그렇지만 위와 같은 프로그램은 스레드를 활용할 이유가 전혀 없습니다! 스레드는 여러개의 작업을 한번에 실행하고 싶을 때에 활용하는 방법인데, 위처럼 순차적으로 실행될 프로그램은 스레드를 사용하면 오히려 복잡해져서 유지보수가 힘들 수 있습니다.
### For문을 활용한 예제
```python
import threading
import time
import datetime
import random

def action(i):
	time.sleep(random.randint(0, 4))
	print(f'{i}) 현재시각.. ' + str(datetime.datetime.now().second) + "초를 지나고 있습니다..\n")

tasks = []

for i in range(10):
    tasks.append(threading.Thread(target=action, args=(i,)))

for task in tasks:
    task.start()

for task in tasks:
    task.join()    
```
0초부터 4초까지 랜덤한 정수초 대기 후, 현재 초를 출력하는 프로그램입니다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzgyOTg1Mjk4XX0=
-->