# arcstudio-i18n
Repositorio central de internacionalizacao (i18n) para os aplicativos da ARC Studio.

## Objetivo
Este repositorio guarda os textos traduziveis usados pelos projetos da ARC Studio.
A ideia e simples:
- Cada idioma fica em um arquivo dentro de `messages/`.
- Todos os arquivos devem seguir o mesmo modelo estrutural.
- Apenas os valores de texto devem mudar entre idiomas.

## Estrutura do repositorio
```text
.
├── locale.json
├── messages/
│   ├── pt-br.json
│   ├── en-us.json
│   └── es-es.json
└── README.md
```

## Como o carregamento de locale funciona
Arquivo: `locale.json`

Exemplo atual:
```json
{
  "meta": {
    "type": "locales",
    "defaultLang": "en-us",
    "version": "1.0.0"
  },
  "locales": "messages/{locale}.json"
}
```

Significado:
- `meta.type`: tipo de configuracao (atualmente `locales`).
- `meta.defaultLang`: locale padrao quando nao houver locale selecionada.
- `meta.version`: versao do formato do arquivo.
- `locales`: mascara de caminho para os arquivos de idioma.
- `{locale}`: placeholder que sera substituido pelo codigo do idioma (ex.: `pt-br`, `en-us`, `es-es`).

Observacao importante:
- Com o modelo atual (`"locales": "messages/{locale}.json"`), voce nao precisa cadastrar manualmente cada idioma em `locale.json`.
- Basta criar o arquivo correto em `messages/` usando o codigo esperado.

## Formato obrigatorio de `messages/*.json`
Cada arquivo de idioma deve manter esta estrutura raiz:
- `meta`
- `messages`

Exemplo minimo:
```json
{
  "meta": {
    "name": "english (United States)",
    "author": {
      "name": "Seu Nome",
      "url": "https://seu-site.com"
    }
  },
  "messages": {
    "web": {},
    "auth": {}
  }
}
```

Regra principal:
- O arquivo `messages/pt-br.json` e o modelo estrutural.
- Novas locales devem copiar a estrutura dele exatamente.

## Regras obrigatorias de traducao
- Nao renomear chaves.
- Nao remover chaves.
- Nao adicionar chaves novas sem alinhamento com o time.
- Nao mudar tipo de valor (string continua string, array continua array, objeto continua objeto).
- Nao quebrar placeholders como `{date}`, `{org}`, `{count, plural, ...}`.
- Nao quebrar tags inline como `<bold>...</bold>` ou `<arc_studio>...</arc_studio>`.
- Traduzir somente os textos (valores string), mantendo tokens tecnicos intactos.

## Tutorial: adicionar uma nova locale
Exemplo: adicionar frances (`fr-fr`).

1. Escolha o codigo da locale
- Padrao recomendado: minusculo com hifen, ex.: `fr-fr`, `de-de`, `it-it`.

2. Crie o arquivo novo a partir do modelo
```bash
cp messages/pt-br.json messages/fr-fr.json
```

3. Edite o bloco `meta`
Arquivo: `messages/fr-fr.json`
- Ajuste `meta.name` para o nome do idioma.
- Atualize `meta.author.name` e `meta.author.url` se necessario.

4. Traduza todos os textos
- Traduza valores em `messages.*` para o idioma novo.
- Mantenha todas as chaves e a mesma ordem logica.
- Nao altere placeholders/tags.

5. Ajuste comentarios de rota (se usados)
Se o arquivo tiver comentarios como:
```jsonc
// https://arcstudio.online/pt-br
```
Troque para:
```jsonc
// https://arcstudio.online/fr-fr
```

6. Verifique se o JSON continua valido no seu fluxo
- O workspace esta configurado para permitir comentarios `//` em `.json` no editor.
- Se o runtime consumidor nao aceitar comentarios, remova comentarios antes de publicar, ou use parser com suporte a JSONC.

7. Confirme se a locale pode ser resolvida pela mascara
Com `"locales": "messages/{locale}.json"`, `fr-fr` sera resolvida como:
- `messages/fr-fr.json`

## Tutorial: atualizar uma locale existente
1. Abra o arquivo desejado em `messages/`.
2. Edite apenas textos traduziveis.
3. Nao altere estrutura nem placeholders.
4. Revise termos consistentes com o restante da plataforma.

## Erros comuns
- Traduzir placeholders por engano.
- Apagar uma chave sem perceber.
- Mudar array para objeto (ou vice-versa).
- Traduzir nomes de chaves em vez de traduzir somente os valores.
- Esquecer de atualizar `meta.name` da nova locale.

## Checklist antes de enviar
- Existe arquivo novo em `messages/<locale>.json`.
- Estrutura esta igual ao `messages/pt-br.json`.
- Placeholders e tags estao intactos.
- Textos estao realmente no idioma alvo.
- Comentarios de rota (se existirem) estao corretos.

## Convencao de nomes
- Use nomes de arquivo em minusculo: `pt-br.json`, `en-us.json`, `es-es.json`.
- Use a mesma locale em links/comentarios internos quando aplicavel.

## Licenca
Este projeto esta sob a licenca definida em `LICENSE`.
