#Este projeto foi criado simplesmente para dar um exemplo do funcionamento da Fila de Tarefas no Laravel 6.x

##Primeiramente vamos entender o que são os [Jobs][1] no Laravel
[1]: https://laravel.com/docs/6.x/queues#creating-jobs "Jobs"
Os jobs nada mais são do que um controller normal que é chamado através do comando `NomeDoJob::dispatch()`.
Para criar um job execute o comando no terminal `php artisan make:job NomeDoJob`, depois ele irá aparecer no diretório `SuaAplicacao\app\Jobs`. Ao clicar no job irão aparecer dois métodos, o primeiro é o `__construct` que irá ser executado quando o job for chamado e outro que é o `handle` que é de fato onde grande parte da lógica do seu job irá ficar e será executada.
##Como trabalhar com as [Filas](https://laravel.com/docs/6.x/queues "Filas") do Laravel
Primeiramente caso queira que após um job ser dispachado colocar ele em uma fila, precisamos rodar o comando `php artisan queue:table` e depois `php artisan migrate` que irá criar as migrations necessárias (serão criadas duas tabelas uma para os jobs e a outra para os jobs que falharam) para se armazenar no banco de dados e mudar o nosso `.env` e no lugar de `QUEUE_CONNECTION=sync` coloque `QUEUE_CONNECTION=database` depois para que sejam executados os jobs que estão na tabela `jobs` basta colocar a fila para ser executada com o comando `php artisan queue:work`.
###Como isso funciona?
Sempre que usamos `dispatch` em um `job`, este será [serializado](https://pt.wikipedia.org/wiki/Serializa%C3%A7%C3%A3o "serializado") no banco de dados e quando colocarmos a fila para ser executada ele é deserealizado do banco e executado.
####Exemplo nesse código do projeto
Toda vez que criamos um novo usuário no controller `laravel_queue\app\Http\Controllers\Auth\RegisterController.php` no método `create` usamos o `dispatch` do `job` `RegisterUser`.
Caso resolva testar, certifique-se de que o comando `php artisan queue:work` esteja em execução.
###Agradecimentos
[ Leandro Henrique Reis](https://www.youtube.com/watch?v=I0bNbG-lzA8 " Leandro Henrique Reis")
[Felipe Braiani](http://brtechsistemas.com.br "Felipe Braiani")