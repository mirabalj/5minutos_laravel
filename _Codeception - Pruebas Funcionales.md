Creo un fichero de pruebas:

`php codecept.phar generate:cept functional HomePage`



```
filename: tests/functional/HomePageCept.php


$I = new FunctionalTester($scenario);
$I->wantTo('View Home Page');

// When
$I->amOnPage('/');
// Then
$I->seeInTitle('Salud Digital Formación');
$I->see('div[id=superior]');
```

Ejecuto la prueba:

`codecept run functional`
