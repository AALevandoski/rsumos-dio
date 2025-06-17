# rsumos-dio

## Implantação de Máquinas Virtuais (VMs) no Azure

### O que é uma VM?

Uma máquina virtual é um recurso de computação no Azure que emula um computador físico, permitindo executar sistemas operacionais e aplicativos como se fosse uma máquina local.

É possível escolher o sistema operacional, o tamanho da VM (memória e CPU), os discos, a rede, e outras configurações.

### Componentes principais de uma VM

- Imagem do sistema operacional (Windows, Ubuntu, etc.)
- Tamanho da VM (define a capacidade e o custo)
- Disco do sistema operacional
- Discos adicionais (opcionais)
- Rede virtual (VNet) e sub-rede
- Grupo de segurança de rede (NSG)
- IP público (se for necessário acesso externo)

### Passo a passo pelo portal do Azure

1. Acessar o portal: https://portal.azure.com
2. Ir até "Máquinas Virtuais" > "Criar"
3. Preencher as informações: nome da VM, região, SO, credenciais de acesso
4. Escolher o tamanho da VM (ex: B1s para testes)
5. Configurar discos, rede, regras de acesso
6. Revisar as configurações e criar

### Exemplo com Azure CLI

Para criar uma VM básica com Ubuntu usando CLI:

bash
az vm create \
  --name MinhaVM \
  --resource-group MeuGrupoDeRecursos \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --size Standard_B1s

Esse comando cria a VM e gera chaves SSH automaticamente.
Observação: o grupo de recursos precisa existir. Se necessário, crie com az group create.

--------------------------------------------
#Desanexando Disco de Máquina
Por que desanexar?

Desanexar um disco pode ser necessário para liberar espaço, reduzir custos, reutilizar o disco em outra VM, ou realizar manutenções. É uma boa prática remover discos que não estão em uso, já que continuam gerando cobrança. é possível desanexar discos com a VM ligada, desde que o sistema operacional suporte.

O disco do sistema operacional não pode ser desanexado sem deletar a VM.
Sempre revise os recursos no portal para evitar cobranças com discos não utilizados.
O Azure permite o uso de scripts, CLI ou PowerShell para automatizar o gerenciamento.
Discos desanexados podem ser reutilizados em outras VMs ou convertidos em imagens.

Passo a passo pelo portal do Azure

   1- Acesse a VM pelo portal
   2- Clique em "Discos" no menu lateral
   3- Identifique o disco que não é o do sistema operacional
   4- Clique em "Desanexar" (ou "Detach")
   5- O disco será desanexado, mas ainda existirá como recurso isolado

Listar os discos anexados:
az vm show \
  --name MinhaVM \
  --resource-group MeuGrupoDeRecursos \
  --query "storageProfile.dataDisks"

Desanexar o disco:
az vm disk detach \
  --name nomeDoDisco \
  --resource-group MeuGrupoDeRecursos \
  --vm-name MinhaVM

