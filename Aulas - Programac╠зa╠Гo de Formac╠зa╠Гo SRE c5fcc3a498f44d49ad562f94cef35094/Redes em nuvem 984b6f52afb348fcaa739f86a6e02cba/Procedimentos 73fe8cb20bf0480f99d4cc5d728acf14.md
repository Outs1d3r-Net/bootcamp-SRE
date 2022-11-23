# Procedimentos

---

## Criando sua primeira VPC

---

- [ ]  Entre em uma conta AWS que não é utilizada para nada de produção.
- [ ]  Vá em Virtual Private Cloud (VPC)
- [ ]  Seleciona a região que deseja (sugiro uma não seja utilizada normalmente)
- [ ]  Delete a VPC padrão existente
- [ ]  Clique em Criar VPC
- [ ]  Defina o nome: VPC-Estudo-<Seu Nome>
- [ ]  Escolha um bloco de rede como: 10.xx.0.0/16 | Exemplo: 10.10.0.0/16
- [ ]  Vá em Subnets
- [ ]  Clique em Criar sub-rede
- [ ]  Selecione sua VPC criada anteriormente
- [ ]  Crie suas sub-redes no padrão passado na Aula

```markdown
2 Públicas: 10.10.0.0/24 - Pub-AZ-A, 10.10.1.0/24 - Pub-AZ-B
2 Privadas: 10.10.2.0/24 - Priv-AZ-A, 10.10.3.0/24 - Priv-AZ-B
```

- [ ]  Ajuste na configuração para que as públicas obtenham IP público ao serem inicializadas
- [ ]  Vá em internet Gateway e crie um
- [ ]  Atrele ele, a sua VPC criada anteriormente
- [ ]  IGW-Estudos-<Seu Nome>
- [ ]  Vá em Route Tables
- [ ]  Criar tabela de rotas, com o nome Publicas
- [ ]  Selecione a VPC criada anteriormente
- [ ]  Crie uma rota direcionando 0.0.0.0/0 para o seu Internet Gateway criado anteriormente
- [ ]  Defina essa tabela de roteamento como a padrão (Default)
- [ ]  Vá no serviço de NAT Gateways e crie um gateway do tipo de conectividade Público → NAT-GW-Estudos-<Seu Nome>
- [ ]  Volte em Route Tables
- [ ]  Criar tabela de rotas, com o nome Privadas
- [ ]  Selecione a VPC criada anteriormente
- [ ]  Crie uma rota direcionando 0.0.0.0/0 para o seu NAT Gateway
- [ ]  Atrele suas sub-redes às tabelas de roteamento respectivas

## **Teste**

---

- [ ]  Crie um EC2 em sua rede pública e dê um comando de ping para o google: ping 8.8.8.8
- [ ]  Crie um security group junto de nome Testes-<seu-nome>-estudos, e libere SSH (porta 22) para o IP da sua casa
- [ ]  Crie agora um EC2 dentro de sua rede privada
- [ ]  Atrele o mesmo security group, porém agora você precisa liberar que o próprio security group possa acessar ele mesmo através da porta SSH (22)
- [ ]  Através do primeiro EC2 (público), acesso o segundo EC2 (Privado), e veja se retorna teste ping no google: ping 8.8.8.8
- [ ]  Se sim, pronto, suas sub-redes públicas e privadas saem para a internet, sem exposição de serviços críticos para a internet (entrada)