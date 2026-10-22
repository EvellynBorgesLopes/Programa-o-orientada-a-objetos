class BancoCentral{
    // static significa que não precisa instanciar para usar -> usar new
   static transferirValor(emitente, destinatario, valor, senha) {

        if (emitente.sacar(valor,senha) == true) {
        destinatario.depositoViaBc(valor)
    }}

}


class Pessoa{
    #nome
    #cpf
    #dataNasc
    #endereco

    get nome () {
        return this.#nome
    }
    
    constructor(cpf, nome, dataNasc)
    {
       this.#cpf = cpf
       this.#nome = nome
       this.#dataNasc = dataNasc       
    }

    criarConta(){
        return new Conta(this)
    }
}


class Conta {
    #senha = "admin"
    #dono
    #saldo

    constructor(dono) {
        this.#saldo = 0;
        this.#dono = dono
    }

    // get sempre retorna algo
    get saldo() { 
        return this.#saldo;
    }

    // set é pra atualizar o valor
    set saldo(valor) {
        this.#saldo += valor;
    }

    verificarSenha(verificandoSenha) {
        return this.#senha === verificandoSenha;
    }

    depositoViaBc(saldo) {
        this.#saldo += saldo
    }

    depositar(valor, senha) { //sobrecarga 

        if (!this.verificarSenha(senha)) {
            console.log("Senha incorreta");
            return;
        }

        if (valor < 0) {
            console.log("Valor depositado não pode ser negativo");
            return;
        }

        if (valor > 5000) {
            console.log(
                "Valor do depósito excedeu o valor máximo permitido. Dirija-se ao caixa."
            );
            return;
        }

        this.#saldo += valor;

        console.log(`Depósito de R$ ${valor} realizado.`);
    }

    sacar(valorSaque, senha) {

        if(valorSaque <= 0)
        {
            console.log("Não é possivel sacar um valor negativo")
            return false
        }

        if (!this.verificarSenha(senha)) {
            console.log("Senha incorreta");
            return false
        }

        if (this.#saldo - valorSaque >= 0) {
            this.#saldo -= valorSaque;

            console.log(`Saque de R$ ${valorSaque}`);
            return true
        } 
        else {
            console.log("Saldo insuficiente");
            return false
        }
    }

    extrato(senha) {

        if (!this.verificarSenha(senha)) {
            console.log("Senha incorreta");
            return;
        }

        console.log(`${this.#dono.nome}, Seu saldo é = ${this.#saldo}`);
    }
}


// Criando as pessoas

let Jose = new Pessoa(
    126365,
    "José Silva",
    "10/10/88"
);

let Manoel = new Pessoa(
    123654,
    "Manoel Costa",
    "10/10/10"
);


// Criando as contas

ContaJose = Jose.criarConta();

ContaManoel = Manoel.criarConta();

contaCopia = ContaJose
contaCopia2 = contaCopia

// Operações na conta do José

ContaJose.depositar(5000, "admin");

ContaJose.sacar(1000, "admin");

ContaJose.extrato("admin");


// Usando o SET

ContaJose.saldo = 200;

console.log("Saldo atual:", ContaJose.saldo);

ContaJose.saldo = 100;

console.log("Saldo atual:", ContaJose.saldo);


// Transferência



// Mostrando os saldos

console.log("Saldo José:", ContaJose.saldo);

console.log("Saldo Manoel:", ContaManoel.saldo);


// Extratos


ContaJose.extrato("admin");

ContaManoel.extrato("admin");


ContaJose.extrato("admin");
ContaManoel.extrato("admin");
contaCopia.extrato("admin");

BancoCentral.transferirValor(ContaJose, ContaManoel, 300, "admin")

contaCopia.depositar(1000, "admin")


console.log("")
ContaJose.extrato("admin");
ContaManoel.extrato("admin");
contaCopia.extrato("admin");
contaCopia2.extrato("admin")# Programa-o-orientada-a-objetos
