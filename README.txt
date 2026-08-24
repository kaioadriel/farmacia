CONTROLE DE FARMÁCIAS — ENTREGA ATUALIZADA

BANCO DE DADOS E AUTENTICAÇÃO
- O Firebase Authentication foi preservado.
- O Cloud Firestore continua sendo o banco de dados principal e autoritativo.
- As leituras em tempo real com onSnapshot e as gravações no Firestore continuam ativas.
- O cache persistente oficial do Firestore foi habilitado para permitir consulta e sincronização quando a conexão oscilar. Ele não substitui o banco Firebase.

NOVOS CAMPOS E AUTOPREENCHIMENTO
- O cadastro de paciente agora possui Clínica Principal e Médico Principal.
- Ao selecionar uma farmácia em uma nova compra, o app procura a compra mais recentemente salva para aquela farmácia.
- Se houver um paciente válido nesse registro, ele é selecionado automaticamente.
- Em seguida, Clínica Principal e Médico Principal do paciente são preenchidos na compra.
- A troca manual do paciente também atualiza a clínica e o médico com seus dados principais.

CORREÇÕES TÉCNICAS APLICADAS
- A instalação PWA agora usa manifest e service worker reais, em arquivos próprios.
- As bibliotecas e os ícones necessários à interface foram incluídos localmente no pacote, sem depender dos CDNs originais.
- A proteção contra conteúdo HTML/JavaScript inserido em cadastros foi reforçada, incluindo validação dos anexos de imagem.
- Datas de hoje passaram a respeitar a data local do aparelho, evitando mudança de dia causada por UTC.
- O valor monetário é interpretado e reaberto corretamente no padrão brasileiro, inclusive valores salvos com ponto decimal no Firestore.
- A numeração de compras antigas passou a atualizar somente o código necessário, sem sobrescrever o restante do registro.
- A restauração do backup valida os registros antes de gravar e usa um lote atômico do Firestore dentro do limite seguro informado pelo app.
- Relatórios omitem metadados técnicos e mostram os nomes de Clínica Principal e Médico Principal.

INSTALAÇÃO COMO PWA
1. Publique a pasta completa em um endereço HTTPS. Abrir somente index.html não fornece instalação PWA nem service worker.
2. Mantenha index.html, manifest.json, service-worker.js, icons/ e vendor/ na mesma estrutura de pastas.
3. Abra o endereço no Chrome e use a opção Instalar aplicativo.
4. O primeiro login e o carregamento inicial dos dados exigem conexão. Depois disso, o Firestore mantém cache local e sincroniza alterações pendentes quando a internet voltar.

REGRAS DO FIRESTORE
- O arquivo firestore.rules limita os dados de /users/{uid}/... ao próprio UID autenticado e ao e-mail já autorizado pelo app.
- O arquivo não altera o projeto Firebase sozinho. Para aplicar a proteção no servidor, publique a regra pelo console do Firebase ou execute, em um projeto Firebase CLI configurado:
  firebase deploy --only firestore:rules
- Confirme o e-mail autorizado em firestore.rules antes de publicar caso ele seja alterado no app.

ARQUIVOS PRINCIPAIS
- index.html: aplicativo completo.
- index.txt: cópia idêntica do index.html.
- manifest.json, service-worker.js e icons/: instalação PWA.
- vendor/: bibliotecas locais para a interface funcionar sem depender dos CDNs originais.
- firestore.rules e firebase.json: regra de segurança pronta para publicação no projeto Firebase.

OBSERVAÇÃO SOBRE FIREBASE
- A configuração original, o login e o banco Firestore não foram removidos.
- Para login Google, o domínio em que o app for publicado precisa continuar autorizado no Firebase Authentication.
