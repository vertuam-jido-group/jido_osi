# jido_osi

Imagem pronta do **Jidō OS** — microkernel bare-metal AArch64 (inspirado em seL4)
para veículo autônomo, validado em silício real numa **Raspberry Pi 4** (BCM2711).

Este repositório existe só pra distribuir a imagem já compilada, pra quem quiser
baixar e testar numa RPi4. O código-fonte completo é privado.

## O que já está validado no hardware (v1.2.0)

- Boot bare-metal (EL2→EL1), MMU, GICv2, SMP (4 cores)
- Pipeline crítico em EL0 (percepção → controle → SafetyMonitor → atuadores) + safe-stop
- WCET medido no Cortex-A72 real (single-core e multi-core)
- Storage persistente (JidoFS numa partição dedicada do SD/EMMC)

## Como testar

### 1. Baixe

- `jido_os_rpi4_vv1.2.0.img.gz`
- `jido_os_rpi4_vv1.2.0.img.gz.sha256`

### 2. Confira a integridade

```sh
shasum -a 256 -c jido_os_rpi4_vv1.2.0.img.gz.sha256
```

### 3. Grave no cartão SD

**Raspberry Pi Imager** (mais fácil): "Choose OS" → "Use custom" → selecione o
`.img.gz` → escolha o cartão → "Write".

**Linha de comando:**

```sh
gunzip jido_os_rpi4_vv1.2.0.img.gz
# macOS (diskutil list p/ achar o device; diskutil unmountDisk antes):
sudo dd if=jido_os_rpi4_vv1.2.0.img of=/dev/rdiskN bs=4m status=progress
# Linux:
sudo dd if=jido_os_rpi4_vv1.2.0.img of=/dev/sdX bs=4M status=progress
```

### 4. Ligue a Raspberry Pi 4

Console serial via USB-TTL **3,3 V** (nunca 5 V) nos pinos GND/TXD/RXD, 115200 8N1
(o firmware entrega a UART pronta). O shell (`jido>`) responde a `help`, `ps`,
`wcet`, `test`, entre outros.

## Aviso honesto de escopo

Isto é um projeto de pesquisa em estágio inicial (ASIL-B no carrinho de testes,
não ASIL-D veicular). Roda hoje só na Raspberry Pi 4 — não é o hardware final do
veículo. Use por conta própria; não é software certificado para uso em via pública.
