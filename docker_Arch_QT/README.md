# Docker Arch Linux + Qt Development Environment

Este projeto fornece um ambiente de desenvolvimento Qt baseado em Arch Linux com acesso via VNC.

## Características

- **Base**: Arch Linux (mais recente)
- **Ferramentas Qt**: Qt5 base e ferramentas de desenvolvimento
- **Compilador**: GCC
- **Interface Gráfica**: XFCE4
- **Acesso Remoto**: VNC Server
- **Resolução padrão**: 1280x800

## Pré-requisitos

- Docker instalado e funcionando
- Cliente VNC (VNC Viewer, TightVNC, etc.)

## Como usar

### 1. Construir a imagem

```bash
docker build -t arch-qt-dev .
```

### 2. Executar o container

```bash
docker run -d -p 5901:5901 --name qt-dev-env arch-qt-dev
```

### 3. Conectar via VNC

- **Endereço**: `localhost:5901`
- **Senha**: `vncpassword`

Use seu cliente VNC favorito para conectar.

## Comandos úteis

### Parar o container
```bash
docker stop qt-dev-env
```

### Iniciar container existente
```bash
docker start qt-dev-env
```

### Acessar terminal do container
```bash
docker exec -it qt-dev-env bash
```

### Remover container
```bash
docker rm qt-dev-env
```

## Desenvolvimento Qt

Após conectar via VNC:

1. Abra o terminal no XFCE4
2. Use `qmake` para gerar Makefiles
3. Use `make` para compilar projetos
4. Ferramentas disponíveis: `qt5-qmake`, `designer`, `linguist`

### Exemplo de projeto Qt

```bash
# Criar um projeto simples
mkdir meu_projeto_qt
cd meu_projeto_qt

# Criar main.cpp
cat > main.cpp << 'EOF'
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    QLabel hello("Hello Qt!");
    hello.show();
    return app.exec();
}
EOF

# Criar arquivo .pro
echo "QT += widgets" > projeto.pro
echo "SOURCES = main.cpp" >> projeto.pro

# Compilar
qmake
make

# Executar
./projeto
```

## Personalização

### Alterar senha do VNC

Modifique a linha no Dockerfile:
```dockerfile
&& echo "nova_senha" | vncpasswd -f > /home/devuser/.vnc/passwd \
```

### Alterar resolução

Modifique o comando CMD no Dockerfile:
```dockerfile
CMD ["vncserver", ":1", "-geometry", "1920x1080", "-depth", "24"]
```

## Solução de problemas

### Container não inicia
- Verifique se a porta 5901 não está em uso
- Use `docker logs qt-dev-env` para ver logs

### Não consegue conectar via VNC
- Confirme que o container está rodando: `docker ps`
- Teste a conexão: `telnet localhost 5901`

### Performance lenta
- Aumente recursos do Docker
- Use resolução menor no VNC

## Pacotes instalados

- `base-devel`: Ferramentas de desenvolvimento
- `qt5-base`: Bibliotecas Qt5 core
- `qt5-tools`: Ferramentas Qt (Designer, etc.)
- `gcc`: Compilador C/C++
- `tigervnc`: Servidor VNC
- `xfce4`: Ambiente desktop
- `xfce4-goodies`: Aplicações extras XFCE4

## Contribuição

Para contribuir com melhorias:

1. Fork este repositório
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Envie um Pull Request

## Licença

Este projeto está sob licença MIT. Veja o arquivo LICENSE para detalhes.
