# ÜcretsizVDS
Bunun üzerine Türkçe kaynak bulamadığım için size ücretsiz şekilde VDS oluşturmayı anlatacağım.

Öncelikle bize gerekli kaynaklar sadece 
NGrok sitesi; https://ngrok.com/
ve gerekli kodlar;

name: CI
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: windows-latest
    steps:
    - name: Download
      run: Invoke-WebRequest https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-windows-amd64.zip -OutFile ngrok.zip
    - name: Extract
      run: Expand-Archive ngrok.zip
    - name: Auth
      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      env:
        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
    - name: Enable TS
      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0
    - run: Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
    - run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
    - run: Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "Test123." -Force)
    - name: Create Tunnel
      run: .\ngrok\ngrok.exe tcp 3389

----------------------------------------------------------------------------------------------------------------------------------------------
Bundan sonra yapmamız gerekenlere geçiyoruz;
  - Öncelikle ngrok sitesine girip hesap oluşturuyoruz,
  - Hesaba giriş yaptıktan sonra soldaki menüden Getting Started > Your Authtoken kısmına giriyoruz
![image](https://github.com/ertesizpazar/-cretsizVDS/assets/141847656/ce6deb04-5b33-44ec-9fee-3b0fca471906)
  -
