 las trust pueden ser automaticas o especificamnente establecidas
 una relacion de trust es entre dos dominios o dos forest
las trust tienen atribuos como 
-Trust direction
	-(one way)-> los usuarios de
	-Bidirectional->


tipos de trust

| tipo           | descripcion                                                                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| one way        | usuarios de un domain pueden acceder resources de otro                                                                                            |
| two way        | usuarios de dos dominios relacionados pueden acceder resources del otro                                                                           |
| transitive     | se da en bidirectional trusts si A<->B<->C, entonces A<->C                                                                                        |
| Non transitive |                                                                                                                                                   |
| Parent-child   | siempre seran two way y transitivas (relaciones de herencia)                                                                                      |
| Tree-root      | siempre seran two way y transitivas (relaciones entre raices de arboles de dominios)                                                              |
| external trust | trust entre dos dominios de distintos forest donde al menos uno de los dominios no es forest root, pueden ser one way o 2 way, no son transitivas |
|                | trust entre las forest root de dos forest distintos                                                                                               |
todas las trust default intraforest (tree-root,parent-child) dentro de un mismo forest, son relaciones transitive y two way trust

incluso si un solo dominio es comprometido de un forest, todo el forest se considera comprometido debido a la transitividad