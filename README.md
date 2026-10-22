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
.....
class Usuario{
    #nome
    #cpf
    #saldo
    #limite
    #historico

    constructor(nome, cpf, limite){
        this.#nome = nome
        this.#cpf = cpf
        this.#saldo = 0
        this.#limite = limite
        this.#historico = new Historico()

    }

    get nome(){
        return this.#nome
    }

    get cpf(){
        return this.#cpf
    }

    get saldo(){
        return this.#saldo
    }

    get limite(){
        return this.#limite
    }

    get historico(){
        return this.#historico
    }

    alterar_limite(valor){
        if (valor > this.#limite){
            console.log("Limite excedido")
            return false
        }
        this.#limite -= valor
        return true
    }

    retirar(valor){
        this.#saldo -= valor
        return true
    }

    depositar(valor){
        this.#saldo += valor
        return true
    }



}

class Pessoal extends Usuario{

    constructor(nome, cpf){
        super(nome,cpf, 1000)
    }

}

class Empresarial extends Usuario{

    constructor(nome, cpf){
        super(nome,cpf, 10000)
    }

}

class Historico{
    #historico

    constructor(){
        this.#historico = []
    }

    get historico(){
        return this.#historico
    }

    adicionar(transacao){
        this.#historico.push(transacao)
    }

    listar(){
        console.log("Historico\n")
        for(const i of this.#historico){
            console.log(`origem: ${i.origem.nome}\ndestino: ${i.destino.nome}\nvalor: ${i.valor}\nstatus: ${i.status}`)
        }
    }
}

class Transacao{
    #origem
    #destino
    #valor
    #status

    constructor(origem, destino, valor, status){
        this.#origem = origem
        this.#destino = destino
        this.#valor = valor
        this.#status = status
    }

    get origem(){
        return this.#origem
    }

    get destino(){
        return this.#destino
    }

    get valor(){
        return this.#valor
    }

    get status(){
        return this.#status
    }

}

class Transferencia{
    static transferir(origem, destino, valor){

        // se origem tem saldo suficiente e se o limite de transferencia
        // if(origem.saldo >= valor){
        //     console.log("saldo de origem suficiente")
        // }
        // else{
        //     console.log("saldo insuficiente")
        // }

        // if (origem.limite >=valor)
        
        if (origem.saldo >= valor && origem.limite >= valor){
           console.log("saldo disponivel e limite disponivel")
            if ( origem.retirar(valor) && origem.alterar_limite(valor) && destino.depositar(valor)){
                
                const nova_transacao = new Transacao(origem, destino, valor, "Sucesso")
                origem.historico.adicionar(nova_transacao)
                destino.historico.adicionar(nova_transacao)
                console.log("historico registrado")

            }
            
        }
        else{
            console.log("Saldo ou limite excedido")
            const nova_transacao = new Transacao(origem, destino, valor, "Falha")
            origem.historico.adicionar(nova_transacao)
            console.log("historico registrado")
        }
    }
}

const isa = new Pessoal("isa", "111")
const belli = new Empresarial("belli", "222")

isa.depositar(200)
belli.depositar(40)

Transferencia.transferir(isa,belli,50)

isa.historico.listar()
belli.historico.listar()
......

