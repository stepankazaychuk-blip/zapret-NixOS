# Zapret Systemd Service for NixOS

Конфигурация автозапуска сервиса **Zapret** (DPI bypass) для операционной системы NixOS с использованием обёртки от *Sergeydigl3*.

## 📋 Исходный код конфигурации

Добавьте этот блок кода в ваш конфигурационный файл NixOS (например, в `configuration.nix` или отдельный модуль):

```nix
#zapret
systemd.services.zapret-auto = {
  description = "Zapret (Sergeydigl3 Wrapper) DPI bypass service";
  after = [ "network.target" ];
  wantedBy = [ "multi-user.target" ];
  
  path = with pkgs; [ 
    iptables nftables gawk iproute2 coreutils curl 
    gnugrep gnused bash procps git ncurses 
  ];
  
  serviceConfig = {
    Type = "simple";
    User = "root";
    WorkingDirectory = "/home/user/zapret-discord-youtube-linux";
    Environment = "TERM=xterm";
    ExecStart = pkgs.writeShellScript "zapret-start" ''
      printf "1\n1\nn\nn\n1\n11\n" | /home/user/zapret-discord-youtube-linux/service.sh
    '';
  };
};
#n-пробел, цифры-выбор конфига zapret
```

## 🛠 Описание параметров

* **Автоматический ввод:** Конструкция `printf "1\n1\nn\nn\n1\n11\n"` автоматически выбирает нужные пункты в интерактивном меню скрипта `service.sh` при запуске.
* **Зависимости:** В секции `path` указаны все системные утилиты, необходимые для корректной работы правил фильтрации трафика (`iptables`, `nftables` и др.).

## 🚀 Важное при установке

Перед применением конфигурации **обязательно измените пути** `/home/user/...` в строках `WorkingDirectory` и `ExecStart` на ваши реальные пути к папке с проектом (замените `user` на ваше имя пользователя).

После сохранения файла обновите систему:
```bash
sudo nixos-rebuild switch
```



