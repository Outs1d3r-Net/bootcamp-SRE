# Código

---

## Criar uma Aplicação React

---

```bash
npx create-react-app <NOME DA APLICAÇÃO>
```

## Configurar o Buckets Deixando ele Público

---

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::<NOME DO BUKET>/*"
    }
  ]
}
```

## Fazer o Deploy Manual Via CLI

---

```bash
aws s3 sync ./build s3://<NOME DO BUKET> --delete
```

## Validar no Browser

---

```markdown
https://<NOME DO BUKET>.s3.amazonaws.com/index.html
```

## Criar o Workflows do GitHub Actions

---

```yaml
name: main Build
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x]

    steps:
      - uses: actions/checkout@v1
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      - name: npm Install
        run: |
          npm install
      - name: app Build
        run: |
          npm run build
      - name: Deploy to S3
        uses: jakejarvis/s3-sync-action@master
        with:
          args: --acl public-read --delete
        env:
          AWS_S3_BUCKET: ${{ secrets.AWS_BUCKET_NAME }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ${{ secrets.AWS_REGION }}
          SOURCE_DIR: "build"
```