---
layout: post
title:  "Programmation fonctionnelle en Java"
categories: JAVA FONCTIONNELLE
date:   2020-07-24 19:18:25 +0200
---
Historiquement la **programmation fonctionnelle** en Java n'a pas été facile, et plusieurs aspects  n'étaient pas possibles avec Java.

Avec Java 8 , Oracle a fait un effort pour faciliter la **programmation fonctionnelle**, cet effort était couronné avec succès en quelque sorte. Dans ce tutoriel sur la **programmation fonctionnelle** en Java, je vais passer en revue les bases et les possibilités qu'elle offre.

## Notions de la programmation fonctionnelle:

  
La programmation fonctionnelle contient les concepts clefs ci-dessous :

-   **Fonction de première classe (Functions as first class objects)**
    
-   **Fonctions pures (Pure functions)**
    
-   **Fonction d'ordre supérieur (Higher order functions)**
    

La programmation purement fonctionnelle a également un ensemble de règles à suivre :

-   **Pas d'État (No state)**
    
-   **Pas d'effets secondaires (No side effects)**
    
-   **Variables immuables (Immutable variables)**
    
-   **Favoriser la récursivité au lieu des boucles**
    

  

Ces concepts et règles seront expliqués tout au long du reste de ce tutoriel même si vous ne suivez pas toutes ces règles en permanence, vous pouvez toujours bénéficier des concepts de la programmation fonctionnelle dans vos applications. Comme vous le verrez, la programmation fonctionnelle n'est pas adaptée à tous les problèmes. En particulier, l'idée de **"pas d'effets secondaires"** rend difficile, par exemple, l'écriture dans une base de données (c'est un effet secondaire). Vous devez apprendre quels sont les problèmes que la programmation fonctionnelle peut résoudre, et ceux qu'elle ne peut pas résoudre.

  

## Fonction de première classe (Functions as first class objects)

  

Le paradigme de la programmation fonctionnelle considère les fonctions comme des objets de première classe dans le langage. Cela signifie que vous pouvez créer une "instance" d'une fonction, tout comme une référence variable vers cette instance de la fonction, tout comme une référence à une chaîne de caractères ou tout autre objet. Les fonctions peuvent également être passées comme paramètre à d'autres fonctions.

  

En Java, les méthodes se distinguent des objets classiques. Ce qui s'en rapproche le plus, ce sont les expressions lambda de Java. **(Je ne traiterai pas ici les expressions lambda en Java.)**

  
  
  

## Fonctions pures

  

Une fonction est pure si :

-   L'exécution de la fonction n'a pas d'effets secondaires.
    
-   La valeur de retour de la fonction dépend uniquement des paramètres d'entrée passés à la fonction.
    


Voici un exemple de fonction (méthode) pure en Java :

  
{% highlight java %}
	public class ObjetAvecFonctionPure{
		public int somme(int a, int b) {
			return a + b;
		}
	}
{% endhighlight %}

  

Notez que la valeur de retour de la fonction **somme()** ne dépend que des paramètres d'entrée. Remarquez également que la fonction **somme()** n'a pas d'effets secondaires, ce qui signifie qu'elle ne modifie aucun état (variable) en dehors de la fonction.

  

Contrairement à cela, voici un exemple de fonction non-pure :

  

{% highlight java %}
	public class ObjetAvecFonctionNonPure{
		private int valeur = 0;
		public int somme(int autreValeur) {
			this.valeur += autreValeur;
			return this.valeur;
		}
	}
{% endhighlight %}
  

Remarquez comment la méthode **somme()** utilise une variable membre à la **class** pour calculer sa valeur de retour, et qu'elle modifie également l'état de la valeur de la variable membre, ce qui est un effet secondaire.

  

## Fonction d'ordre supérieur (Higher order functions)

Une fonction est d'ordre supérieur si au moins L'une de ces conditions est remplie :

-   La fonction prend une ou plusieurs fonctions comme paramètres.
    
-   La fonction renvoie une autre fonction comme résultat.
    

En Java, ce qui se rapproche le plus d'une fonction d'ordre supérieur correspond à une fonction/méthode qui prend une ou plusieurs **expressions lambda** comme paramètres et renvoie une autre expression lambda. Voici un exemple de fonction d'ordre supérieur en Java :

  
{% highlight java %}
	public class ClassOrdreSuperieur {
		public <T> IFactory<T> createFactory(IProducer<T> producer, IConfigurator<T> configurator) {
			return () -> {
				T instance = producer.produce();
				configurator.configure(instance);
				return instance;
			}
		}
	}
{% endhighlight %}
  

Remarquez comment la méthode **createFactory()** renvoie une expression lambda comme résultat. Ceci représente la première condition d'une fonction d'ordre supérieur.

  

Notez également que la méthode **createFactory()** prend comme paramètres deux instances qui sont toutes les deux des implémentations des interfaces **(IProducer et IConfigurator)**. En effet, les expressions lambda de Java devront implémenter une interface fonctionnelle, vous vous souvenez ?

  

Imaginez les interfaces comme ceci :

  
{% highlight java %}
	public interface IFactory<T> {
		T create();
	} 
	public interface IProducer<T> {
		T produce();
	}
	public interface IConfigurator<T> {
		void configure(T t);
	}
{% endhighlight %}

  

Comme vous le voyez, toutes ces interfaces sont de nature fonctionnelle, elles peuvent donc être implémentées par des expressions lambda Java - et la méthode **createFactory()** est par conséquent une fonction d'ordre supérieur.

  

Les fonctions d'ordre supérieur sont traitées avec des exemples différents dans la section sur les Fonctions d'ordre supérieur.

  

## Pas d'État (No State)

  

Comme mentionné précédemment, le paradigme de la programmation fonctionnelle a pour principe ne pas avoir d'état. "Pas d'état", signifie généralement qu'il n'y a pas d'état extérieur à la fonction. Une fonction peut avoir des variables locales contenants un état temporaire en interne, en revanche la fonction ne peut pas faire référence aux variables membres de la classe ou de l'objet dont elle fait partie.

  

Voici un exemple de fonction qui n'utilise aucun état extérieur :

  

{% highlight java%}
	public class Calculateur {
		public int somme(int a, int b) {
			return a + b;
		}
	}
{% endhighlight %}

Contrairement à ce dernier, voici un exemple de fonction qui se sert de l'état extérieur :

{% highlight java%}
	public class Calculateur {
		private int initVal = 5;
		public int somme(int a) {
			return initVal + a;
		}
	}
{% endhighlight %}

Cette fonction ne respecte pas la règle du "Pas d'Etat".

  

## Pas d'effets secondaires ( No Side Effects)

  

Une autre règle dans le paradigme de la programmation fonctionnelle est l'absence d'effets secondaires. Cela signifie qu'une fonction ne peut changer aucun état en dehors d'elle même. Le changement d'état en dehors d'une fonction est appelé un effet secondaire.

  

L'état qui se trouve en dehors d'une fonction désigne à la fois les variables membres de la classe ou de l'objet correspondant à la fonction et les variables propres aux paramètres des fonctions ou l'état dans des systèmes externes comme les systèmes de fichiers ou les bases de données.

  

## Variables immuables (Immutable variables)

  

Une troisième règle dans le paradigme de la programmation fonctionnelle est celle des variables immuables. Les variables immuables permettent d'éviter facilement les effets secondaires.


## Favoriser la récursivité au lieu des boucles
  

Une quatrième règle dans le paradigme de la programmation fonctionnelle est de favoriser la récursivité plutôt que les boucles. La récursivité utilise des appels de fonction pour effectuer des boucles, de manière à ce que le code devient fonctionnel.

  

Une autre alternative aux boucles est l'API Java Streams. Cette API est faite par les fonctions.

  

## Interface fonctionnelle (Functional Interfaces)

  

Une interface fonctionnelle en Java est une interface qui ne possède qu'une seule méthode abstraite. On entend par méthode abstraite une seule méthode qui n'est pas implémentée. Une interface peut avoir plusieurs méthodes, par exemple des méthodes par défaut et/ou des méthodes statiques, toutes les deux avec des implémentations mais tant que l'interface n'a qu'une seule méthode qui ne soit pas implémentée, l'interface est considérée comme fonctionnelle.

  

Voici un exemple d'interface fonctionnelle :

  
{% highlight java %}
	public interface MonInterface {
		public void run();
	}
{% endhighlight %}

Voici un autre exemple d'interface fonctionnelle avec une méthode par défaut et une méthode statique :

  
{% highlight java %}
	public interface MonInterface2 {
	
		public void run();
		public default void doIt() {
			System.out.println("en réalisation");
		}
		public static void doItStatically() {
			System.out.println("en réalisation statique");
		}
	}
{% endhighlight %}

  

Notez les deux méthodes avec leurs implémentations. Il s'agit toujours d'une interface fonctionnelle car seule run() n'est pas implémentée (abstraite). Cependant, s'il y avait davantage de méthodes sans implémentation, l'interface ne serait plus une interface fonctionnelle et ne pourrait donc pas être implémentée par une expression lambda Java.