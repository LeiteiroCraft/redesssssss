import ipaddress

def calcular_subrede(classe_ip, num_subredes):
    if classe_ip == 'A':
        bits_rede = 8
        total_ips = 2 ** (32 - bits_rede)
    elif classe_ip == 'B':
        bits_rede = 16
        total_ips = 2 ** (32 - bits_rede)
    elif classe_ip == 'C':
        bits_rede = 24
        total_ips = 2 ** (32 - bits_rede)
    elif classe_ip == 'D' or classe_ip == 'E':
        raise ValueError("Classe D e E não são usadas para sub-redes.")
    else:
        raise ValueError("Classe IP inválida. Deve ser A, B ou C.")

    bits_subrede = (num_subredes - 1).bit_length() 
    nova_mascara_bits = bits_rede + bits_subrede
    total_subrede_ips = 2 ** (32 - nova_mascara_bits)
    ips_utilizaveis = total_subrede_ips - 2  
    mascara_binaria = '1' * nova_mascara_bits + '0' * (32 - nova_mascara_bits)
    mascara_decimal = str(ipaddress.IPv4Address(int(mascara_binaria, 2)))
    mascara_cidr = f"/{nova_mascara_bits}"

    return {
        'total_ips': total_ips,
        'ips_utilizaveis': ips_utilizaveis,
        'mascara_decimal': mascara_decimal,
        'mascara_binaria': mascara_binaria,
        'mascara_cidr': mascara_cidr,
        'bits_subrede': bits_subrede,
        'nova_mascara_bits': nova_mascara_bits
    }
    

def calcular_endereco_rede_broadcast(ip, mascara_cidr):
    rede = ipaddress.ip_network(f"{ip}/{mascara_cidr}", strict=False)
    return {
        'endereco_rede': str(rede.network_address),
        'endereco_broadcast': str(rede.broadcast_address),
        'faixa_ips_utilizaveis': (str(rede.network_address + 1), str(rede.broadcast_address - 1))
    }

def ip_para_binario(ip_decimal):
    return '.'.join([format(int(octeto), '08b') for octeto in ip_decimal.split('.')])

def validar_ip(ip):
    try:
        ipaddress.ip_address(ip)
        return True
    except ValueError:
        return False

def main():
    print("Calculadora de Sub-Redes\n")

    while True:
        classe_ip = input("Digite a classe do IP (A, B, C): ").upper()
        if classe_ip in ['A', 'B', 'C']:
            break
        print("Classe IP inválida. Por favor, digite A, B ou C.")
        

    num_subredes = int(input("Digite a quantidade de sub-redes desejada: "))
    resultado = calcular_subrede(classe_ip, num_subredes)

    print(f"\nResultados para a Classe {classe_ip}:")
    print("\n")
    print(f"Total de IPs na rede: {resultado['total_ips']}")
    print("\n")
    print(f"IPs utilizáveis por sub-rede: {resultado['ips_utilizaveis,  \n O Fabio e bucetudo']}")
    print("\n")
    print(f"Máscara de sub-rede (decimal): {resultado['mascara_decimal\n Voce soca e coda fofo']}")
    print("\n")
    print(f"Máscara de sub-rede (binária): {resultado['mascara_binaria\n BusetinhaSuja\n']}")
    print("\n")
    print(f"Máscara de sub-rede (CIDR): {resultado['mascara_cidr \n Fabio voce e Gay']}")
    print("\n")
    
    while True:
        ip_decimal = input("\nDigite um endereço IP em decimal de acordo com a rede selecionada: ")
        if validar_ip(ip_decimal):
            break
        print("IP inválido. Por favor, digite um IP válido.")
        print("\n")
        
    ip_binario = ip_para_binario(ip_decimal)
    print(f"IP em binário: {ip_binario}")

    rede_broadcast = calcular_endereco_rede_broadcast(ip_decimal, resultado['nova_mascara_bits'])
    print(f"Endereço de rede: {rede_broadcast['endereco_rede']}")
    print("\n")
    print(f"Endereço de broadcast: {rede_broadcast['endereco_broadcast']}")
    print("\n")
    print(f"Faixa de IPs utilizáveis: {rede_broadcast['faixa_ips_utilizaveis'][0]} - {rede_broadcast['faixa_ips_utilizaveis'][1]}")
    print("\n")
    
if __name__ == "__main__":
    main()
