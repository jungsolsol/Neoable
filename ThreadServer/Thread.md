# Thread ?
프로세스내의 실행되는 흐름의 단위
python의 Mutli - Threading은 I/O Bound Program에 사용하기 적합함


# 데몬 쓰레드 / 일반 쓰레드

메인 쓰레드를 제외한 모두가 데몬 쓰레드
메인 쓰레드에의해 만들어지는 쓰레드는 모두 일반쓰레드이다 (생성한 쓰레드의 상태를 상속받음)

데몬쓰레드는 일반쓰레드의 작업을 돕는 보조적 역할을 담당한다
일반쓰레드가 종료되면 데몬쓰레드도 강제종료된다