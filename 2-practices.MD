
## 2. Pratiques d'ingénierie logicielle : Test-Driven Development, Refactoring, Continuous Integration et Continuous Delivery.


### TDD (Test-Driven Development)

Inventé par [Kent Beck](https://fr.wikipedia.org/wiki/Kent_Beck) (connu aussi pour la creation de la methode agile XP extreme programming), le TDD a été popularisé dans les années 2000 et est considéré depuis par la communauté des clean coders comme une bonne pratique de developpement. Il permet de garantir la qualité du code et accelere la velocité d'une équipe.

Avant de commencer ce point je pense qu'il est indispensable de définir ce qu'est le code de test et de production. 
Le code de production est le code qui est nécessaire pour que l'application se comporte comme exigé dans les business requirments.
Le code de test est le code qui va tester le code de production.

Avec le TDD nous pouvons couvrir 100% du code. Nous verrons les outils pour mesurer cette couverture dans un autre chapitre.

#### TDD Nindo

**RED-GREEN-REFACTOR** Le principe general consiste à faire des cycles iteratifs de developpement en 3 étapes:
  - Ecrire un test qui compile mais echoue
  - Ecrire le code de production pour que le test passe
  - Refactoriser le code (production et test sans en changer le comportement)

**Pas plus que necessaire** Il faut noter que le code de production doit etre minimaliste, c'est a dire qu'il doit strictement repondre aux specifications fonctionnelles rien de plus.

**Un test a la fois** Il faut s'assurer qu'un test passe avant d'en écrire un autre.

**Independance** Il faut que les tests soient independants les un des autres. Aucune dependance, de contexte ou d'état des tests précedents

**Refacto** Il faut refactoriser le code régulierement et s'assurer que les tests passent toujours

