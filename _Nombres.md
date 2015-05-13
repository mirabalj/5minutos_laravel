###Interfaces:

No usar la palabra 'Interface'.

```
// Haz esto
interface Translator

//No hagas esto
interface TranslatorInterface
```
---
No usar la palabra 'Default' y especificar siempre qué hace a esa clase diferente.

```
//Haz esto
class LanguageTranslator implements Translator

//No hagas esto
class Translator implements Translator
class DefaultTranslator implements Translator
```
---
No uses `-nameable`

```
// Haz esto
class Product implements CastsToJson, HasTimestamp

// No hagas esto
class Product implements Jsonable, Timestampable
```
---
Respetar el contrato del interface:

```
<?php
interface Animal {
    public function makeNoise();
}
class Dog implements Animal {
    public function makeNoise() {}
    public function fetchStick() {}
}
// elsewhere:
public function myClient(Animal $animal) {
    $animal->fetchStick();
}
```
No llames a métodos que no están declarados en la interface que estás utilizando. (`fetchStick()` está declarado en `Dog` y no en `Animal`.

---
Usar las interfaces como roles:
```
<?php
class Teacher implements User {}
class Pupil implements User {}
class Parent implements User {}
```
---
Regla para definir una interface:

Si imaginas varias formas de hacer lo mismo, usa una interface, sino, usa una clase.


---
Signal to noise ratio.

Tener más porcentaje de palabras con significado que palabras de la implementación (Factory, Repository, Builder, Composite, Decorator...)

---
###More info:
http://verraes.net/2013/09/sensible-interfaces/
http://cyrille.martraire.com/2012/09/whats-your-signal-to-noise-ratio-in-your-code/
http://verraes.net/2014/06/named-constructors-in-php/
http://verraes.net/2013/10/verbs-in-class-names/

----
#Todo:
http://verraes.net/2014/06/when-to-use-static-methods-in-php/  
http://verraes.net/2014/08/dry-is-about-knowledge/   
http://www.jamesshore.com/Agile-Book/ubiquitous_language.html   
http://www.giorgiosironi.com/2010/02/3-simple-steps-to-ubiquitous-language.html   
