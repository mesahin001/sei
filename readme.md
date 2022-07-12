Asagidaki kodlarla o andaki aktif kullanicilarin listesini peer dosyasina yazabiliyoruz.

PEERS=$(curl -sS http://rpc2.bonded.zone:21157/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{printf $1":"$(NF)} { printf(",") }'| sed 's/.$//')
sed -i "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/;" $HOME/.sei/config/config.toml

sudo systemctl restart seid && sudo journalctl -u seid -f -o cat
