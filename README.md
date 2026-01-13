# Como instalar/configurar/usar as `partições` no `Linux Ubuntu`

## Resumo

Neste documento estão contidos os principais comandos e configurações para instalar/configurar/usar as `partições` no `Linux Ubuntu`.

## _Abstract_

_This document contains the main commands and settings to install/configure the `partitions` on `Linux Ubuntu`._


## Descrição [1]

### Partições

Partições são segmentos do disco rígido usados para separar diferentes tipos de dados e facilitar a gestão do sistema. Cada partição funciona como uma seção independente, permitindo que o sistema operacional, os aplicativos e os dados do usuário sejam armazenados separadamente. Isso não só melhora a organização dos dados, mas também pode aumentar a segurança e a eficiência do sistema.

No `Ubuntu`, as partições mais comuns são:

- **A partição `/boot/efi`:** é usada em sistemas com firmware UEFI para armazenar os arquivos de inicialização do sistema operacional.

- **A partição `/boot`:** armazena os arquivos necessários para inicializar o sistema operacional.

- **A partição `/tmp`:** é um diretório temporário usado pelo sistema e pelos programas para armazenar arquivos temporários durante a execução.

- **A partição `/var`:** é usada para armazenar dados variáveis, como `logs` do sistema, `spools` de impressão e outros arquivos que podem crescer ou mudar de tamanho durante a operação do sistema.

- **A partição raiz:** (representada como "`/`"), onde o sistema operacional é instalado;

- **A partição `home`:** ("`/home`"), que armazena os arquivos pessoais dos usuários;

- **A partição `swap`:** usada para a gestão de memória.

### `luks`

`LUKS` (Linux Unified Key Setup) é uma especificação de criptografia de disco completo amplamente utilizada no `Linux`. Ele permite criar volumes criptografados, fornecendo uma camada adicional de segurança para os dados armazenados no disco. O `LUKS` gerencia chaves de criptografia, permitindo o acesso seguro aos dados somente após a autenticação bem-sucedida do usuário. É uma ferramenta valiosa para proteger informações confidenciais em sistemas `Linux`.

### `lvm`

`LVM` (Logical Volume Manager) é uma tecnologia de gerenciamento de armazenamento flexível para sistemas `Linux`. Permite criar volumes lógicos que podem ser redimensionados facilmente, independentemente do tamanho do disco físico subjacente. Com o LVM, os administradores de sistema podem criar, redimensionar e mover volumes lógicos sem interromper o sistema ou perder dados. Isso proporciona maior flexibilidade e eficiência na alocação e gerenciamento de espaço em disco.


## 1. Configurar/Instalar/Usar as `partições` no `Linux Ubuntu` (caso ainda não esteja instalado) **SEM** criptografia [2]

Para configurar/instalar/usar as `partições` no `Linux Ubuntu`, siga as configuraçõea abaixo (de prefeência do menor armazenamento para o MAIOR):

| Número | Size (MB)   | Type of the new partition | Local for the new partition | Use as                        | Mount point       | Tipo    | Início da partição | Final da partição | Observação(ções) |
|:------:|:-----------:|:-------------------------:|:---------------------------:|:-----------------------------:|:-----------------:|:--------|:-------------------|:------------------|:-----------------|
| 1      | `512`       | `Primary`                 | `Beginning of this space`   | `EFI System Partition`        | `/boot/efi`       | `fat32` | `1MiB`             | `512MiB`          | N/A              |
| 2      | `512`       | `Primary`                 | `Beginning of this space`   | `Ext4 journaling file system` | `/boot`           | `ext4`  | `512MiB`           | `1025Mib`         | N/A              |
| 3      | `2048`      | `Primary`                 | `Beginning of this space`   | `Ext4 journaling file system` | `/tmp`            | `ext4`  | Analisar conforme decisão de incluir ou **NÃO** a partição | Analisar conforme decisão de incluir ou **NÃO** a partição | Caso seja criada esta partição, o que pode **NÃO** ser recomendado, é recomendado que cada SO ter sua partição `/tmp`, por exemplo, `/tmpubuntu` e `/tmpkali` |
| 4      | `10240`     | `Primary`                 | `Beginning of this space`   | `Ext4 journaling file system` | `/var`            | `ext4`  | Analisar conforme decisão de incluir ou **NÃO** a partição | Analisar conforme decisão de incluir ou **NÃO** a partição | Caso seja criada esta partição, o que pode **NÃO** ser recomendado, é recomendado que cada SO ter sua partição `/tmp`, por exemplo, `/varubuntu` e `/varkali` |
| 5      | Igual à memória RAM física do computador para poder habilitar o `hibernate corretamente` ou, pelo menos, `1024` | `Primary`                   | `Beginning of this space`     | `swap area`/`linux-area`    | `/swap`  | `ext4`  | `1025MiB` | `33025MiB` (baseado em um computador com memória RAM de `32GB`, ajustar para outros tamanhos de memória se for o caso) | N/A              |
| 6      | `262144`    | `Primary`                   | `Beginning of this space`     | `Ext4 journaling file system`                      | `/`             | `ext4`  | `33025MiB` (baseado em um computador com memória RAM de `32GB`, ajustar para outros tamanhos de memória se for o caso) | `295170MiB` | **NÃO** criar como `/root` |
| 7      | Restante    | `Logical`                   | `Beginning of this space`     | `Ext4 journaling file system`                      | `/home`         | `ext4`  | `295170MiB` | Até o final do HD/SSD | N/A |


### 1.1 Sobre as partições `/tmp` e `/var`. 

<div style="margin-left: 2em;">

Compartilhar as mesmas partições `/tmp` e `/var` entre diferentes instalações do Linux, como o Ubuntu e o Kali Linux, não é recomendado e pode levar a problemas significativos.

Diferenças de Configuração e Versão: Ubuntu e Kali podem ter diferentes configurações...

Segurança e Isolamento: O compartilhamento desses diretórios compromete...

</div>


### 1.2 Sobre o **NÃO** compartilhamento de Sistemas Operacionais (SO) com as partições `/tmp` e `/var`

<div style="margin-left: 2em;">

Compartilhar as mesmas partições `/tmp` e `/var` entre diferentes instalações do Linux, como o Ubuntu e o Kali Linux, não é recomendado e pode levar a problemas significativos. Aqui estão as razões:

Diferenças de Configuração e Versão: Ubuntu e Kali podem ter diferentes configurações, versões de software e requisitos de sistema, o que pode causar conflitos se ambos tentarem acessar e modificar os mesmos arquivos em `/tmp` ou /var.

Segurança e Isolamento: O compartilhamento desses diretórios compromete o isolamento entre os sistemas operacionais, aumentando o risco de problemas de segurança. Por exemplo, um sistema pode ser afetado por arquivos temporários maliciosos ou corrompidos deixados pelo outro.

Gerenciamento de Pacotes e Logs: `/var` contém dados importantes relacionados ao gerenciamento de pacotes e logs do sistema. Compartilhá-lo entre diferentes distribuições pode resultar em confusões sobre quais pacotes estão instalados, atualizados ou removidos, além de misturar logs de ambos os sistemas.

Atualizações e Consistência: Durante as atualizações, cada sistema pode alterar arquivos em `/var` ou `/tmp` de maneira que possa não ser compatível com o outro sistema. Isso pode resultar em instabilidade ou falhas ao iniciar.

Se você precisa de isolamento e estabilidade (o que geralmente é o caso), mantenha `/tmp` e `/var` separados para cada instalação. Caso haja necessidade de compartilhar algum dado entre os sistemas, considere usar uma partição de dados separada exclusivamente para esse propósito, evitando compartilhar diretórios críticos ao sistema. Se realmente precisar compartilhar recursos entre sistemas, faça-o de maneira controlada e com pleno conhecimento dos possíveis riscos e implicações.

</div>

### 1.3 Criar as partições `/tmp` e `/var` depois da instalação

<div style="margin-left: 2em;">

Se inicialmente você não criou partições separadas para `/tmp` e `/var` em sua instalação do Ubuntu, mas posteriormente decide que deseja tê-las, você pode sim criar e mover esses diretórios para novas partições. Para que o Ubuntu reconheça e comece a usar as novas partições para `/tmp` e `/var`, você deve:

1. **Iniciar o Linux Live:** Você já deve ter feito isso para acessar seu sistema a partir de um ambiente Live.

2. Criar as novas partições `/tmp` e `/var` usando uma ferramenta de particionamento `gparted`.

3. Formatar essas partições com o sistema de arquivos desejado.

4. **Identificar as novas partições:** Use o comando `lsblk` ou `fdisk -l` para identificar as novas partições. Anote os nomes de dispositivo, como `/dev/sdaX` e `/dev/sdaY`, onde `X` e `Y` são os números das partições para `/tmp` e `/var`, respectivamente.

5. **Montar a partição raiz atual:** Você precisa montar a partição raiz do seu sistema Ubuntu para acessar os diretórios atuais `/tmp` e `/var`. Supondo que `/dev/sda1` seja a sua partição raiz:

    ```bash
    sudo mkdir /mnt/ubuntu
    sudo mount /dev/sda1 /mnt/ubuntu
    ```

6. **Montar as novas partições:** Agora, monte as novas partições em diretórios temporários para copiar os dados. Por exemplo:

    ```bash
    sudo mkdir /mnt/newtmp
    sudo mkdir /mnt/newvar
    sudo mount /dev/sdaX /mnt/newtmp
    sudo mount /dev/sdaY /mnt/newvar
    ```

7. **Copiar os dados:** Use o comando `rsync` para copiar os dados de `/tmp` e `/var` para as novas partições. Isso preserva as permissões e a estrutura de diretórios:

    ```bash
    sudo rsync -av /mnt/ubuntu/tmp/ /mnt/newtmp/
    sudo rsync -av /mnt/ubuntu/var/ /mnt/newvar/
    ```

8. **Atualizar o `fstab`:** Antes de reiniciar no sistema normal, você deve atualizar o arquivo `/etc/fstab` para refletir as mudanças.

    8.1 **Edite o arquivo `/mnt/ubuntu/etc/fstab`:** `sudo nano /mnt/ubuntu/etc/fstab` 


    8.2 **Adicione as entradas para as novas partições, algo como:**

    ```bash
    /dev/sdaX `/tmp` ext4 defaults 0 0
    /dev/sdaY `/var` ext4 defaults 0 0
    ```

    Substitua `/dev/sdaX` e `/dev/sdaY` pelos identificadores corretos das suas partições e ajuste o tipo de sistema de arquivos se não for `ext4`.

9. Para desmontar a partição `/mnt/ubuntu`, você pode usar o comando `umount` no terminal. Aqui está o comando que você precisa executar: `sudo umount /mnt/ubuntu`

10. **Reiniciar:** Após completar esses passos, reinicie seu computador e remova o disco ou pen drive do `Linux Live`. Se tudo foi configurado corretamente, o sistema deve iniciar normalmente e usar as novas partições para `/tmp` e `/var`.

Lembre-se de fazer backup dos seus dados antes de realizar essas operações para evitar perda de dados.

</div>

## 1. Configurar/Instalar/Usar as `partições` no `Linux Ubuntu` (caso ainda não esteja instalado) **COM** criptografia [2]

<div style="margin-left: 2em;">

Para configurar/instalar/usar as `partições` no `Linux Ubuntu`, siga as configuraçõea abaixo (de prefeência do menor armazenamento para o MAIOR):

| Número | Size (MB)   | Type of the new partition | Local for the new partition | Use as                        | Mount point       | Tipo       | Início da partição | Final da partição     | Observação(ções) |
|:------:|:-----------:|:-------------------------:|:---------------------------:|:-----------------------------:|:-----------------:|:-----------|:-------------------|:----------------------|:-----------------|
| 1      | `512`       | `Primary`                 | `Beginning of this space`   | `EFI System Partition`        | `/boot/efi`       | `fat32`    | `1MiB`             | `512MiB`              | N/A              |
| 2      | `512`       | `Primary`                 | `Beginning of this space`   | `Ext4 journaling file system` | `/boot`           | `ext4`     | `512MiB`           | `1025Mib`             | N/A              |
| 3      | Restante    | `Primary`                 | `Beginning of this space`   | `Ext4 journaling file system` | `/sda3_crypt`     | `Unknown`  | `1025MiB`          | Até o final do HD/SSD | N/A              |
| 4      | Igual à memória RAM física do computador para poder habilitar o `hibernate corretamente` ou, pelo menos, `1024` | `Primary`                   | `Beginning of this space`     | `swap area`/`linux-area`    | `/swap`  | `ext4`  | `1025MiB` | `33025MiB` (baseado em um computador com memória RAM de `32GB`, ajustar para outros tamanhos de memória se for o caso) | N/A              |
| 5      | `262144`    | `Primary`                   | `Beginning of this space`     | `Ext4 journaling file system`                      | `/`             | `ext4`  | `33025MiB` (baseado em um computador com memória RAM de `32GB`, ajustar para outros tamanhos de memória se for o caso) | `295170MiB` | **NÃO** criar como `/root` |
| 6      | Restante    | `Logical`                   | `Beginning of this space`     | `Ext4 journaling file system`                      | `/home`         | `ext4`  | `295170MiB` | Até o final do HD/SSD | N/A |

</div>

## 2. Formatar e criar as partições usando o `Terminal Emulator` do `Linux Ubuntu`

### 2.1 Formatar um disco pelo `Terminal Emulator` do `Linux Ubuntu`

<div style="margin-left: 2em;">

Para formatar um disco no Linux, você normalmente precisará de dois passos principais: criar uma partição no disco e, em seguida, criar um sistema de arquivos nessa partição. Aqui está um guia geral sobre como fazer isso usando ferramentas comuns de linha de comando como fdisk ou `parted` para criar partições e mkfs para formatar essas partições.

1. **Identificar o Disco**: Primeiro, é importante confirmar qual disco você deseja usar. Você pode listar todos os dispositivos de armazenamento com o seguinte comando:

    ```bash
    sudo lsblk
    ```

2. **Criar Partição no Disco**: Você pode usar fdisk (para discos menores ou sistemas mais antigos) ou `parted` (recomendado para discos maiores ou para um uso mais avançado). Vou usar `parted` para um exemplo com um disco `/dev/sda`. Usando parted:

    ```bash
    sudo parted /dev/sda --script mklabel gpt  # Cria uma tabela de partição GPT
    sudo parted /dev/sda --script mkpart primary ext4 1MiB 100%  # Cria uma partição primária que ocupa todo o disco
    ```

3. **Desmontar a Partição**: Quando você souber o ponto de montagem, por exemplo, `/dev/sda1` ou algum outro diretório, use o comando umount para desmontá-lo:

    ```bash
    sudo umount /dev/sda1
    ```

4. **Formatar a Partição**: Depois de criar a partição, você pode formatá-la com um sistema de arquivos. O tipo de sistema de arquivos que você escolhe dependerá de suas necessidades (por exemplo, `ext4`, `xfs`, `ntfs` etc.).

    4.1 **Formatar como `ext4`**: Suponha que a partição criada seja `/dev/sda1`:
    
    ```bash
    sudo mkfs.ext4 /dev/sda1
    ```

**Considerações de Segurança**

- **Backup**: Sempre certifique-se de ter backups dos dados antes de formatar um disco, pois esse processo apagará todos os dados existentes na unidade.

- **Verificação**: Confirme sempre o dispositivo que você está formatando usando o comando `lsblk` ou `fdisk -l` para evitar apagar dados de outra unidade por engano.

**Alternativas de Sistema de Arquivos**

- **Para sistemas Windows (NTFS)**:

    ```bash
    sudo mkfs.ntfs /dev/sda1
    ```

- **Para sistemas de alta performance (XFS)**:

    ```bash
    sudo mkfs.xfs /dev/sda1
    ```

- **Para armazenamento em rede ou servidores (Btrfs)**:
    
    ```bash
    sudo mkfs.btrfs /dev/sda1
    ```

Cada sistema de arquivos tem suas vantagens e é escolhido com base nas necessidades específicas de uso, como desempenho, confiabilidade, e características do sistema operacional que será usado com ele.

</div>

### 2.2 Criar as partições usando o `Terminal Emulator` do `Linux Ubuntu`

1. **Identificar o Disco**: Primeiro, é importante confirmar qual disco você deseja usar. Você pode listar todos os dispositivos de armazenamento com o seguinte comando:

    ```bash
    sudo lsblk
    ```

2. **Criar Tabela de Partições**: Vamos criar uma nova tabela de partições no disco (isso apagará todos os dados existentes no disco, então tenha certeza de que é o disco correto e que todos os dados importantes foram salvos):

    ```bash
    sudo parted /dev/sda --script mklabel gpt
    ```

3. **Criar Partições**: Agora, vamos criar as partições necessárias. Vou criar uma partição para o EFI (necessária para sistemas UEFI), uma para `/boot` (opcional, mas recomendada para gerenciar kernels e inicializações mais facilmente), e uma grande partição para o `LUKS` que abrigará o sistema operacional e os dados.

    3.1 **Criar partição EFI de `512 MB`**:
    
    ```bash
    sudo parted /dev/sda --script mkpart primary fat32 1MiB 512MiB
    sudo mkfs.vfat -F 32 /dev/sda1
    ```

    3.2 **Criar partição `/boot` de `1 GB` (opcional)**:

    ```bash
    sudo parted /dev/sda --script mkpart primary ext4 512MiB 1537MiB
    sudo mkfs.ext4 /dev/sda2
    ```
    
    3.3 **Criar partição `LUKS` (o restante do disco)**:
    
    ```bash
    sudo parted /dev/sda --script mkpart primary 1537MiB 100%
    ```

4. **Configurar `LUKS`**: Vamos configurar `LUKS` na terceira partição criada (suponho que seja `/dev/sda3`):

    ```bash
    sudo cryptsetup luksFormat /dev/sda3
    sudo cryptsetup open /dev/sda3 sda3_crypt
    ```

5. **Configurar `LVM` sobre `LUKS`** Com o volume `LUKS` aberto, configuraremos o `LVM`:

    ```bash
    sudo pvcreate /dev/sda3
    sudo vgcreate vgubuntu /dev/sda3
    sudo lvcreate -L 32G vgubuntu -n swap
    sudo lvcreate -L 256G vgubuntu -n ubuntu_root # partição home para o `Linux Ubuntu`
    sudo lvcreate -L 128G vgubuntu -n kali_root # partição home para o `Kali Linux`
    sudo lvcreate -l 100%FREE -n home vgubuntu # Ajuste para os 100% livre do disco ou conforme o espaço disponível e necessidade
    sudo mkswap /dev/vgubuntu/swap
    sudo mkfs.ext4 /dev/vgubuntu/root
    sudo mkfs.ext4 /dev/vgubuntu/home
    ```
    
**Explicação do Comando**

- **`l 100%FREE`**: Esta opção especifica que o volume lógico deve usar todas as unidades de extensão livres restantes no grupo de volumes. É útil para maximizar o uso do espaço em disco disponível sem ter que calcular o tamanho exato disponível manualmente.

- **`n home`**: Define o nome do volume lógico como home.
vgubuntu: É o nome do grupo de volumes onde o volume lógico será criado.

#### 2.2.1 Alocar partições durante a instalação do `Linux`

Quando você chegar à tela de instalação do `Ubuntu` a partir de um pendrive bootável, você precisará escolher manualmente onde instalar o sistema operacional. Aqui estão os passos que você deve seguir:

1. **Iniciar o Instalador do `Ubuntu`**: Inicie o seu computador a partir do pendrive bootável do `Ubuntu`: Escolha a opção `"Install Ubuntu"` na tela inicial do Live USB.

2. **Seguir o Processo de Instalação**: Continue através do instalador até chegar à etapa `"Installation type"`.

3. **Escolher o Tipo de Instalação**: Quando chegar à tela `"Installation type"`, selecione `"Something else"` para escolher manualmente as partições onde o `Ubuntu` será instalado.

4. **Alocar Partições**: Agora você precisa especificar as partições para o sistema:

    4.1 **Localizar as Partições LVM**

    4.1.1 Você deve ver as partições LVM listadas na interface do instalador, provavelmente como `/dev/mapper/vgubuntu-root`, `/dev/mapper/vgubuntu-home` etc. Se não estiverem visíveis, certifique-se de que o volume LUKS foi aberto (se você reiniciou o computador, pode precisar abrir o LUKS novamente).

    4.2 **Definir Pontos de Montagem**: Para definir os pontos de montagem, você deve criar duas vezes com o mouse no nome da partição e **NÂO** no nome do volue, como segue:

    4.2.1 Selecione `/dev/mapper/vgubuntu-root` para o ponto de montagem `/`. Este será o sistema de arquivos raiz.

    4.2.2 Selecione `/dev/mapper/vgubuntu-home` para o ponto de montagem `/home` para armazenar os arquivos do usuário.

    4.2.3 Selecione `/dev/mapper/vgubuntu-swap` como área de `swap`.

    4.3 **Partição EFI**: Certifique-se de que a partição EFI (normalmente `/dev/sda1`) seja designada como área de boot EFI. Isso é crucial para sistemas que usam UEFI.

5. **Continuar a Instalação**:

    5.1 Depois de configurar os pontos de montagem e outras opções de partição, continue com a instalação.
    
    5.2 O instalador irá configurar o sistema, instalar o GRUB (bootloader) na partição EFI (isso garantirá que o sistema possa iniciar através do UEFI) e completar o processo.

6. **Finalizar e Reiniciar**:

    6.1 Siga as instruções para finalizar a instalação. Quando tudo estiver pronto, você será solicitado a reiniciar o computador.

    6.2 Remova o pendrive quando solicitado e permita que o computador reinicie no novo sistema Ubuntu instalado.

7. **Primeiro Boot**:

    7.1 No primeiro boot, você pode ser solicitado a desbloquear o volume LUKS digitando a senha que você configurou para o LUKS.

    7.2 Uma vez desbloqueado, o Ubuntu deverá iniciar normalmente.

Espero que essas instruções ajudem você a instalar o `Linux Ubuntu` com sucesso nas partições que você preparou! Se tiver mais dúvidas ou precisar de ajuda durante a instalação, sinta-se à vontade para perguntar.

## Referências

[1] OPENAI. ***Partições no linux ubuntu.*** Disponível em: <https://chat.openai.com/c/f72f7628-d9b6-47e2-ad60-15de383e66a8> (texto adaptado). Acessado em: 10/11/2023 10:49.

[2] LH INFORMÁTICA. ***Como particionar corretamente o hd para instalar o linux.*** Disponível em: <https://www.youtube.com/watch?v=-yxKBFh2F9E>. Acessado em: 10/11/2023 10:50.

[3] OPENAI. ***Vs code: editor popular.*** Disponível em: <https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42> (texto adaptado). Acessado em: 16/04/2024 13:48.

[4] OPENAI. ***Criptografia de Disco LUKS.*** Disponível em: <https://chat.openai.com/c/8910b4a0-0cf5-449b-9fb4-14646df6d613> (texto adaptado). Acessado em: 17/04/2024 13:48.

