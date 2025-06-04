# Configuração de Recursos e Dimensionamento de Máquinas Virtuais na Azure

Este tutorial fornece um passo a passo para criar e configurar máquinas virtuais na Microsoft Azure, ajustar seus recursos e gerenciar o dimensionamento conforme as necessidades de carga de trabalho.

## Pré-requisitos

Antes de começar, certifique-se de ter:

- Uma conta ativa no [Azure](https://portal.azure.com).
- Permissões para criar e gerenciar recursos no Azure.
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) ou acesso ao portal Azure.

## Passo 1: Entrando no Azure Portal

1. Acesse o [Portal do Azure](https://portal.azure.com).
2. Faça login com sua conta do Azure.

## Passo 2: Criando uma Máquina Virtual

### Via Portal do Azure

1. No painel do Azure, clique em **Criar um recurso**.
2. Selecione **Máquina Virtual** na lista de serviços.
3. Preencha os campos obrigatórios:
   - **Assinatura**: Escolha a assinatura de pagamento.
   - **Grupo de Recursos**: Escolha um grupo existente ou crie um novo.
   - **Nome da VM**: Escolha um nome para sua máquina virtual.
   - **Região**: Selecione a região geográfica onde a VM será hospedada.
   - **Imagem**: Escolha o sistema operacional da VM (por exemplo, Ubuntu, Windows).
   - **Tamanho**: Selecione a configuração inicial de CPU e memória.

4. Em **Opções de autenticação**, escolha entre:
   - **Chave SSH** (recomendado para Linux).
   - **Senha** (recomendado para Windows).

5. Em **Disco**, selecione o tipo de disco de armazenamento:
   - **SSD Padrão** ou **Premium** (recomendado para alto desempenho).

6. Clique em **Revisar + Criar** e em seguida **Criar**.

### Via Azure CLI

```bash
# Exemplo de comando para criar uma máquina virtual Ubuntu no Azure
az vm create \
  --resource-group NomeDoGrupoDeRecursos \
  --name NomeDaVM \
  --image UbuntuLTS \
  --size Standard_B1s \
  --admin-username azureuser \
  --generate-ssh-keys
```

## Passo 3: Ajustando o Dimensionamento da VM

### Redimensionar via Portal do Azure

1. No portal do Azure, navegue até **Máquinas Virtuais**.
2. Selecione a VM que deseja redimensionar.
3. No menu à esquerda, clique em **Tamanho**.
4. Escolha um novo tamanho de VM com base nas necessidades de CPU e memória.
5. Clique em **Redimensionar**.

### Redimensionar via Azure CLI

```bash
# Exemplo de comando para redimensionar uma VM
az vm resize \
  --resource-group NomeDoGrupoDeRecursos \
  --name NomeDaVM \
  --size Standard_DS2_v2
```

## Passo 4: Configurando Autoescala

A escalabilidade automática permite ajustar o número de instâncias de VM conforme as necessidades da carga de trabalho.

### Configuração de Autoescala via Portal do Azure

1. Navegue até **Máquinas Virtuais** e selecione o grupo de VMs.
2. No painel à esquerda, clique em **Autoescala**.
3. Clique em **+ Adicionar uma regra**.
4. Defina as condições, como:
   - **Métrica**: CPU, memória ou outros recursos.
   - **Ação**: Escalar verticalmente ou horizontalmente.
5. Defina os parâmetros da ação, como o número máximo de instâncias e a quantidade de incremento.
6. Clique em **Salvar**.

### Configuração de Autoescala via Azure CLI

1. Crie uma política de autoescala:

```bash
az monitor autoscale create \
  --resource-group NomeDoGrupoDeRecursos \
  --name NomeDaRegraAutoescala \
  --min-count 1 \
  --max-count 5 \
  --count 2
```

2. Adicione uma regra de escala:

```bash
az monitor autoscale rule create \
  --resource-group NomeDoGrupoDeRecursos \
  --autoscale-name NomeDaRegraAutoescala \
  --condition "Percentage CPU > 75 avg 5m" \
  --scale out 1
```

## Passo 5: Monitoramento e Otimização

Após configurar sua máquina virtual, é essencial monitorar o uso de recursos para garantir que o desempenho atenda às expectativas.

1. No portal do Azure, navegue até **Monitoramento**.
2. Veja as métricas de uso de CPU, memória e disco.
3. Ajuste as configurações de dimensionamento conforme necessário.

## Conclusão

Seguindo esses passos, você poderá configurar máquinas virtuais no Azure, ajustar seus recursos e implementar autoescala para otimizar o uso de recursos. Para mais informações, consulte a [documentação oficial do Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/).

---

Isso cobre o processo desde a criação até o dimensionamento e autoescala de VMs no Azure.
