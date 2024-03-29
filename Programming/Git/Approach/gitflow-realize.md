
# Работа по GitFlow

1) Постоянные ветки
	- Main/Master - основная ветка, которая выкладывается на prod;
	- Develop - рабочая ветка, куда попадают все принятые изменения.

![[Pasted image 20230122132612.png]]

!! НЕЛЬЗЯ делать коммиты в эти ветки напрямую.

2) Ветки feature / bug - создаются для реализации задачи:
	- ветки создаются от develop ветви;
	- называются feture или bug / номер задачи или название задачи;
	- по итогу работы на верке разработчик создает pull request /megre request на ветку develop;
	- после код ревью задача заливается на девелоп ветку.

![[Pasted image 20230122133026.png]]

3) Релизы
	- как только в develop оказывается достаточный для релиза набор задач, от нее создается релизная ветка release/R1;
	- в данную ветку можно делать коммиты - фиксы по результатам тестирования;
	- после завершения тестирования, ветка вливается в master и в develop.

![[Pasted image 20230122134152.png]]

4) Хотфиксы:
	- от ветки master создается ветка hotfix/number
	- на вертке хотфикса делается исправление;
	- после исправления ветка вливается обратно в master, а так же в develop.

![[Pasted image 20230122134636.png]]

**Ключи:**
- [Git](Git)
- [GitFlow](gitflow);
- [Достоинства и недостатки](gitflow-compare);

**Хештеги:** #Programming/Git/GitFlow