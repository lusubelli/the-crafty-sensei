
## 2. Pratiques d'ingénierie logicielle : Test-Driven Development, Refactoring, Continuous Integration et Continuous Delivery.


### TDD (Test-Driven Development)

Inventé par [Kent Beck](https://fr.wikipedia.org/wiki/Kent_Beck) (connu aussi pour la creation de la methode agile XP extreme programming), le TDD a été popularisé dans les années 2000 et est considéré depuis par la communauté des clean coders comme une bonne pratique de developpement. Il permet de garantir la qualité du code et accelere la velocité d'une équipe.

Avant de commencer ce point je pense qu'il est indispensable de définir ce qu'est le code de test et de production. 
Le code de production est le code qui est nécessaire pour que l'application se comporte comme exigé dans les business requirments.
Le code de test est le code qui va tester le code de production.

Avec le TDD nous pouvons couvrir 100% du code. Nous verrons les outils pour mesurer cette couverture dans un autre chapitre.

Dans un mode ideal, nous aurons un Product Owner capable d'ecrire les regles de gestions en bullet points comme dans l'exemple suivant (pour fizzbuzz)
Si ce n'est le case, vous pouvez suggerer en toute transparence qu'il l'aurait peut etre besoin d'un petit coaching agile, mais en attendant c'est a vous d'essayer d'interpreter ses propos en bullet point (et dans votre bienveillance, accompagnez le pour qu'il sache de quelle maniere vous aimeriez qu'il ecrive ses specifications)


#### Exemple pas à pas avec Fizzbuzz :
Écrire une fonction qui prend en entrée un entier n et qui renvoie une chaînes de caractères.
BR1 Si l'entier est divisible par 3, la chaîne doit etre "fizz".
BR2 Si l'entier est divisible par 5, la chaîne doit etre "buzz".
BR3 Si l'entier est divisible par 3 et par 5, la chaîne doit etre "fizzbuzz".
BR4 Si l'entier n'est pas divisible par 3 ni par 5, renvoyer une erreur "out of fizzbuzz scope".

On commence par ecirer le code de test suivant qui verifie la premiere regle de gestion :

```
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

@DisplayName("Test de la fonctionnalite fizzbuzz")
public class FizzBuzzTest {

    @Test
    @DisplayName("Test du use case fizz")
    public void should_be_fizz() {
        FizzBuzz fizzBuzz = new FizzBuzz();
        assertEquals(fizzBuzz.fizzBuzz(3), "fizz");
        assertEquals(fizzBuzz.fizzBuzz(6), "fizz");
        assertEquals(fizzBuzz.fizzBuzz(9), "fizz");
    }
    
}
```
Puis le code de production strictement nécessaire à la compilation :

```
public class FizzBuzz {

    public String fizzBuzz(int n) {
        return null;
    }
    
}
```
Evidemment on execute ! Et les tests échouent. On implemente desormais le code de production nécessaire au bon deroulement du test. Ce qui donne

```
public class FizzBuzz {

    public String fizzBuzz(int n) {
        if (n % 3 == 0) {
            return "fizz";
        }
        return null;
    }
    
}
```

A ce moment la on peut executer les tests qui seront en succes et refactoriser. Ce qui donne

```
public final class FizzBuzz {

    private static final int FIZZ_DIVIDER = 3;

    final String fizzBuzz(final int n) {
        if (n % FIZZ_DIVIDER == 0) {
            return "fizz";
        }
        return null;
    }
    
}
```

Remarquez qu'a la premiere iteration il y a peu d'axes d'amelioration si ce n'est du nommage, de la visibilité ou de la mutabilité.

On peut donc passer à la regle de gestion 2.

```
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

@DisplayName("Test de la fonctionnalite fizzbuzz")
public class FizzBuzzTest {

    @Test
    @DisplayName("Test du use case fizz")
    public void should_be_fizz() {
        FizzBuzz fizzBuzz = new FizzBuzz();
        assertEquals(fizzBuzz.fizzBuzz(3), "fizz");
        assertEquals(fizzBuzz.fizzBuzz(6), "fizz");
        assertEquals(fizzBuzz.fizzBuzz(9), "fizz");
    }
    
    @Test
    @DisplayName("Test du use case buzz")
    public void should_be_buzz() {
        FizzBuzz fizzBuzz = new FizzBuzz();
        assertEquals(fizzBuzz.fizzBuzz(5), "buzz");
        assertEquals(fizzBuzz.fizzBuzz(10), "buzz");
        assertEquals(fizzBuzz.fizzBuzz(25), "buzz");
    }
    
}
```
Evidemment on execute ! Et un seul test échoue celui du buzz. Seulement apres on implemente le code de production
```
public final class FizzBuzz {

    private static final int FIZZ_DIVIDER = 3;

    final String fizzBuzz(final int n) {
        if (n % FIZZ_DIVIDER == 0) {
            return "fizz";
        } else if (n % 5 == 0) {
            return "buzz";
        }
        return null;
    }
    
}
```
On execute, ca fonctionne, on refacto

```
public final class FizzBuzz {

    private static final int FIZZ_DIVIDER = 3;
    private static final int BUZZ_DIVIDER = 5;

    final String fizzBuzz(final int n) {
        if (n % FIZZ_DIVIDER == 0) {
            return "fizz";
        } else if (n % BUZZ_DIVIDER == 0) {
            return "buzz";
        }
        return null;
    }
    
}
```
On peut donc passer à la regle de gestion 3.
```
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

@DisplayName("Test de la fonctionnalite fizzbuzz")
public class FizzBuzzTest {

    @Test
    @DisplayName("Test du use case fizz")
    public void should_be_fizz() {
        FizzBuzz fizzBuzz = new FizzBuzz();
        assertEquals(fizzBuzz.fizzBuzz(3), "fizz");
        assertEquals(fizzBuzz.fizzBuzz(6), "fizz");
        assertEquals(fizzBuzz.fizzBuzz(9), "fizz");
    }
    
    @Test
    @DisplayName("Test du use case buzz")
    public void should_be_buzz() {
        FizzBuzz fizzBuzz = new FizzBuzz();
        assertEquals(fizzBuzz.fizzBuzz(5), "buzz");
        assertEquals(fizzBuzz.fizzBuzz(10), "buzz");
        assertEquals(fizzBuzz.fizzBuzz(25), "buzz");
    }
    
    @Test
    @DisplayName("Test du use case fizzbuzz")
    public void should_be_fizzbuzz() {
        FizzBuzz fizzBuzz = new FizzBuzz();
        assertEquals(fizzBuzz.fizzBuzz(15), "fizzbuzz");
    }
    
}
```

On execute, ca echoue, on implemente le code de prod
```
public final class FizzBuzz {

    private static final int FIZZ_DIVIDER = 3;
    private static final int BUZZ_DIVIDER = 5;

    final String fizzBuzz(final int n) {
        if (n % FIZZ_DIVIDER == 0 && n % BUZZ_DIVIDER == 0) {
            return "fizzbuzz";
        } else if (n % FIZZ_DIVIDER == 0) {
            return "fizz";
        } else if (n % BUZZ_DIVIDER == 0) {
            return "buzz";
        }
        return null;
    }
    
}
```

Ca marche ! on refacto !
```
public final class FizzBuzz {

    private static final int FIZZ_DIVIDER = 3;
    private static final int BUZZ_DIVIDER = 5;

    final String fizzBuzz(final int n) {
        final StringBuilder fizzbuzzBuilder = new StringBuilder();
        if (n % FIZZ_DIVIDER == 0) {
            fizzbuzzBuilder.append("fizz");
        }
        if (n % BUZZ_DIVIDER == 0) {
            fizzbuzzBuilder.append("buzz");
        }
        return fizzbuzzBuilder.toString();
    }
    
}
```

Evidemment il y a plusieurs facons de faire qui seront correctes.
Je vous laisse imaginer l'iteration pour la BR4 ;)

Remarque: nous reviendrons dessus mais il serait interressant de commit entre chaque iteration TDD :)

#### TDD Nindo

**RED-GREEN-REFACTOR** Le principe general consiste à faire des cycles iteratifs de developpement en 3 étapes:
  - Ecrire un test qui compile mais echoue
  - Ecrire le code de production pour que le test passe
  - Refactoriser le code (production et test sans en changer le comportement)

**Pas plus que necessaire** Il faut noter que le code de production doit etre minimaliste, c'est a dire qu'il doit strictement repondre aux specifications fonctionnelles rien de plus.

**Un test a la fois** Il faut s'assurer qu'un test passe avant d'en écrire un autre.

**Independance** Il faut que les tests soient independants les un des autres. Aucune dependance, de contexte ou d'état des tests précedents

**Refacto** Il faut refactoriser le code régulierement et s'assurer que les tests passent toujours

### Refactoring

