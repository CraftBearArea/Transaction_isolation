ACID MeetUp

—Lost Update—
#App1
get bonus_local_1
update manager set bonus=bonus_local_1+1;
#App2
get bonus_local_2
update manager set bonus=bonus_local_2+1;


—Dirty Read—
#App1
SET SESSION tx_isolation='READ-UNCOMMITTED';
START TRANSACTION;
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
update manager set bonus=2;
ROLLBACK;
#App2
SET SESSION tx_isolation='READ-UNCOMMITTED';
START TRANSACTION;
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
SELECT * FROM manager;
COMMIT;

—Non-repeatable Read—
#App1
SET SESSION tx_isolation='READ-COMMITTED';
START TRANSACTION;
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
SELECT * FROM manager;
update manager set bonus=bonus+1;
COMMIT;
#App2
SET SESSION tx_isolation='READ-COMMITTED';
START TRANSACTION;
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
SELECT * FROM manager;
//женя
SELECT * FROM manager;
COMMIT;

—Phantom Read—
#App1
SET SESSION tx_isolation=‘REPEATABLE READ’;
START TRANSACTION;
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
SELECT * FROM manager;
Insert into manager values(4,4);
COMMIT;
#App2
SET SESSION tx_isolation='REPEATABLE READ';
START TRANSACTION;
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
SELECT * FROM manager;
SELECT * FROM manager;
COMMIT;


Полезные ресурсы:
https://ru.wikipedia.org/wiki/%D0%A3%D1%80%D0%BE%D0%B2%D0%B5%D0%BD%D1%8C_%D0%B8%D0%B7%D0%BE%D0%BB%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8_%D1%82%D1%80%D0%B0%D0%BD%D0%B7%D0%B0%D0%BA%D1%86%D0%B8%D0%B9
Особенности postgresql: https://www.postgresql.org/docs/10/static/transaction-iso.html
Довольно неплохо описано картинками: https://arbinada.com/en/node/619
