# Git e GitHub
## Entendendo como o Git funciona (todos os comandos mostrados são feitos no Git Bash)

### SHA1
SHA1 é um algoritmo de encriptação que gera códigos únicos de 40 caracteres. Esses códigos são usados para nomear um arquivo, um commit, etc. O SHA1 é uma forma curta de representar o estado de um arquivo. Sempre será gerado o mesmo código para um mesmo conteúdo de um arquivo, se tivermos dois arquivos com exatamente o mesmo conteúdo, ambos terão o mesmo hash, mas se mudarmos apenas um caractere se quer, teremos um resultado diferente.

Podemos ver o hash gerado através do cmd, usando o comando:
```bash
openssl sha1 arquivo.txt
```
<br />
### Objetos internos do Git (Blobs, trees e commits)

### Blobs
Blobs são objetos que o git usa para armazenar o conteúdo de arquivos. Além do conteúdo do arquivo ele também armazena metadados, que são o tipo do objeto (blob), tamanho do arquivo, "\0" e por último o conteúdo do arquivo.
Devido a esses metadados são gerados códigos diferentes ao aplicar o SHA1 dentro e fora do Git (comando SHA1 do Git: git hash-object). Para obtermos o mesmo resultado é preciso escrever também os metadados no início do conteúdo.

### Trees
As árvores são objetos que armazenam vários blobs, além de seus próprios metadados, para cada blob ela armazena o tipo dele (blob), seu SHA1 e o nome do arquivo. Uma árvore é responsável por organizar a estrutura dos arquivos, por isso além de blobs, ela também pode apontar para outras árvores, da mesma forma que um diretório pode conter outros diretórios.

### Commits
Um commit aponta para uma árvore, para um parente (o commit anterior a ele), armazena também o nome do autor, a mensagem e o timestamp da sua criação. O SHA1 do commit é um resumo de todas essa informações. Ao fazer qualquer tipo de alteração em qualquer arquivo, o hash do blob correspondente será alterado, mudando também o hash da árvore onde ele está e consequentemente o hash do próprio commit. Tudo isso faz com que o Git seja um sistema de versionamento bem seguro, já que não se pode realizar alterações no código de forma despercebida.

## Chave SSH e Token
### Chave SSH
Recentemente o GitHub mudou sua forma de autenticação, anteriormente usávamos o nosso nome de usuário e nossa senha para realizar um push para o GitHub, agora a nova forma de verificação é realizada através de uma chave SSH. Geramos um par de chaves (pública e privada) no nosso computador e depois registramos a chave pública nas configurações do GitHub, assim sempre que realizarmos um push ele já vai saber que se trata da nossa máquina.
Para gerar a chave usamos o seguinte comando:
```bash
ssh-keygen -t ed25519 -C meuemail@mail.com
```
Será gerado um par de chaves dentro do diretório que será exibido.

Depois de registrar a chave pública no GitHub é preciso fazer mais uma configuração na nossa máquina, primeiro ativamos um processo que será responsável por gerenciar essas chaves, o ssh-agent.
```bash
eval $(ssh-agent -s)
```
Depois que o processo estiver em execução, passamos o caminho da nossa chave privada para ele
```bash
ssh-add nossa-chave-privada
```
### Token
A autenticação por token é semelhante a autenticação antiga por usuário e senha, você gera um token nas configurações do github (define quais as permissões dele e prazo de validade) depois quando for fazer o acesso pelo git ele irá mostrar a telinha pedindo para fazer login com o browser o informar o token de acesso, pronto.