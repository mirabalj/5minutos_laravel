###`Detect PSR-0 namespace roots`

Para que PhpStorm pueda configurar correctamente tu proyecto, es recomendable que configuremos los namespaces de `PSR-0`.

Cuando abres un nuevo proyecto Laravel, es posible que PhpStorm automáticamente lo detecte y te muestre el mensaje: `Detect PSR-0 namespace roots`. En ese caso, elige 'configurarlos manualmente'. Y, si no es así, también puedes configurarlos o modificarlos en las opciones: `Settings, Project, Directories`.

Configura los siguientes directorios:

`Tests`:  
- `tests`

`Source Folders`:
- `app`
- `database`
- `config`

`Excluded Folders`:
- `vendor`

`Resource roots`:
- `resources`
- `public\assets`



