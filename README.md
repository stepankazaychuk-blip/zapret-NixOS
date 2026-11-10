#zapret
  systemd.services.zapret-auto = {
    description = "Zapret (Sergeydigl3 Wrapper) DPI bypass service";
    after = [ "network.target" ];
    wantedBy = [ "multi-user.target" ];
    path = with pkgs; [ iptables nftables gawk iproute2 coreutils curl gnugrep gnused bash procps git ncurses ];
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

