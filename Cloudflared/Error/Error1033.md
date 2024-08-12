# I have error 1033 argo tunnel error

# tunnel sering kali mati dan harus dijalankan ulang
# untuk mengatasi tunnel yang sering kali mati gunakan systemd atau Supervisor,agar dapat memastikan bahwa tunnel Cloudflare tetap berjalan dan secara otomatis restart jika terjadi kegagalan.
sudo nano /etc/systemd/system/cloudflared.service // melihat token yang ada di dalam server
sudo journalctl -u cloudflared.service -f // log aktivitas tunnel

# configurasi supervisor
sudo nano /etc/supervisor/conf.d/cloudflared.conf

# check tunnel list
cloudflared tunnel list

# pastikan id tunnel sama dengan CNAME record yang mengarahkan hostname ke tunnel.
# Contoh:
# Type: CNAME
# Name: adinuegroho.my.id
# Target: c563500d-c695-49eb-a7d2-64d6a4e25500.cfargotunnel.com
# jika tidak hapus tunnel dan cert.pem dan buat baru
cloudflared tunnel login 
cloudflared tunnel create adinugroho.my.id

# Perbarui Konfigurasi config.yml lalu jalankan 
# pindah ke direktori .cloudflared untuk edit config.yml
cloudflared tunnel run adinugroho.my.id
# Jalankan perintah berikut untuk menjalankan cloudflared tunnel di background
nohup cloudflared tunnel run adinugroho.my.id &
# Periksa file nohup.out untuk melihat log output
tail -f nohup.out 

sudo systemctl daemon-reload
sudo systemctl restart cloudflared

# hapus tunnel dan configurasinya
cloudflared tunnel delete <TUNNEL_ID>
rm ~/.cloudflared/<TUNNEL_ID>.json