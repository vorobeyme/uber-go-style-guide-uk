# Вказівники на інтерфейси

Вам майже ніколи не знадобиться вказівник на інтерфейс. Ви повинні передавати інтерфейси за значенням,
оскільки дані, що лежать в основі, завжди можуть бути вказівником.

Інтерфейс складається з двох полів:

1. Вказівник на певну інформацію про тип. Він представлений як "тип".
2. Вказівник на дані. Якщо дані містять вказівник, вони зберігаються напряму.
   Якщо дані містять значення, то зберігається вказівник на це значення.

Якщо ви хочете, щоб методи інтерфейсу могли б змінювати базові дані, то вам слід використовувати вказівник.
