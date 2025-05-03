# Mikrotik-ANTI-brute-force-Attack
Маршрутизаторы Mikrotik, работающие под управлением Router OS, широко используются благодаря своей гибкости и функциональности. Однако, как и любое другое сетевое устройство, они подвержены атакам, в том числе и brute-force атакам, направленным на подбор логинов и паролей для доступа через Winbox.

Egnlish:
The security of network equipment is a critical issue, especially when it comes to routers that control access to your network. Mikrotik routers running RouterOS are widely used due to their flexibility and functionality. However, like any other network device, they are susceptible to attacks, including brute-force attacks aimed at selecting logins and passwords for access through Winbox.
# С простыми атаками - нужно бороться и по простому.
И в этом кроется фундаментальная истина кибербезопасности, которую часто упускают из виду, стремясь к сложным и дорогостоящим решениям. Представьте себе замок, окруженный множеством высоких стен, башен и хитроумных ловушек, но с забытой щелью в основании, через которую легко может проскользнуть мелкий воришка. Так и с киберзащитой: самые сложные системы могут быть скомпрометированы через самые элементарные уязвимости.
English:
With simple attacks, you need to fight in a simple way.
And therein lies the fundamental truth of cybersecurity, which is often overlooked in the pursuit of complex and expensive solutions. Imagine a castle surrounded by many high walls, towers and clever traps, but with a forgotten crack in the base through which a petty thief can easily slip. It's the same with cyber defense: the most complex systems can be compromised through the most basic vulnerabilities.

***
### Fail2Ban Winbox (Защита MikroTik Winbox от Brute Force)
Защита MikroTik от перебора пароля (BruteForce) при подключении через Winbox (IP адрес), используя MikroTik Firewall. При каждой неверной попытке ввода пароля через Winbox, MikroTik отправляет в ответ незашифрованный текст «invalid user name or password».
Информация:
Chain: output
Protocol: 6 (tcp)
Src. Port: 8291
Content: invalid user name or password
Eng:
Fail2Ban Winbox (protecting MikroTik Winbox from brute force)
Download MikroTik from password brute force when connecting via Winbox (IP address) using the MikroTik Firewall. Every time a new attempt is entered using Winbox, MikroTik responds to the unregistered text "invalid username or password".
Information:
Chain: output
Protocol: 6 (tcp)
Src. Port: 8291
Content: invalid username or password.
/ip firewall filter add action=jump chain=output comment="F2B Winbox: Jump to Fail2Ban-Destination-IP chain" content="invalid user name or password" jump-target=Fail2Ban-Destination-IP protocol=tcp src-port=8291
<br>
/ip firewall filter add action=add-dst-to-address-list address-list=BlackList address-list-timeout=60m chain=Fail2Ban-Destination-IP comment="3 Attempt --> BruteForceList" dst-address-list=LoginFailure02
<br>
/ip firewall filter add action=add-dst-to-address-list address-list=LoginFailure02 address-list-timeout=2m chain=Fail2Ban-Destination-IP comment="2 Attempt --> LoginFailure02" dst-address-list=LoginFailure01
<br>
/ip firewall filter add action=add-dst-to-address-list address-list=LoginFailure01 address-list-timeout=1m chain=Fail2Ban-Destination-IP comment="1 Attempt --> LoginFailure01"
<br>
**Add block RAW rules:**
/ip firewall raw
<br>
add action=drop chain=prerouting comment=";; Block Blacklist" \``
    ``src-address-list=BruteForceList
    <br>



