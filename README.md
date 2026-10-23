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
class Usuario{
    #nome
    #cpf
    #quantidade_emprestados
    #limite
    #historico

    constructor(nome, cpf, limite){
        this.#nome = nome
        this.#cpf = cpf
        this.#quantidade_emprestados = 0
        this.#limite = limite
        this.#historico = new Historico()
    }

    get nome(){
        return this.#nome
    }

    get cpf(){
        return this.#cpf
    }

    get limite(){
        return this.#limite
    }
    
    get quantidade_emprestados(){
        return this.#quantidade_emprestados
    }

    get historico(){
        return this.#historico
    }

    emprestar(quantidade){
        if (quantidade > this.#limite || this.#quantidade_emprestados + quantidade > this.#limite){
            console.log("Não é possivel emprestar livros")
            return false;
        }

        this.#quantidade_emprestados += quantidade
        console.log("Livro emprestado")

        return true
    }

    devolver(quantidade){
        if (quantidade > this.#quantidade_emprestados || quantidade < 0){
            console.log("Não é possivel devolver essa quantidade")
            return false
        }

        this.#quantidade_emprestados -= quantidade
        console.log("livro devolvido")
        return true
    }
  
}

class Aluno extends Usuario{
    #matricula   
    // #quantidade_emprestados

    constructor(nome, cpf, matricula){
        super(nome, cpf, 2)
        this.#matricula = matricula
    }

    get matricula(){
        return this.#matricula
    }
    
}

class Professor extends Usuario{
    #departamento

    constructor(nome, cpf, departamento){
        super(nome, cpf, 5)
        this.#departamento = departamento
    }

    get departamento(){
        return this.#departamento
    }

}


class Livro{
    #isbn
    #titulo
    #autor
    #status

    constructor(isbn, titulo, autor){
        this.#isbn = isbn
        this.#titulo = titulo
        this.#autor = autor
        this.#status = "disponivel"
    }

    get isbn(){
        return this.#isbn
    }

    get titulo(){
        return this.#titulo
    }

    get autor(){
        return this.#autor
    }

    get status(){
        return this.#status
    }

    set status(tipo){
        this.#status = tipo
        // console.log("status alterado")
    }
}

class Biblioteca{
    #livros

    constructor(){
        this.#livros = []
    }

    cadastrar_livro(livro){
        this.#livros.push(livro)
    }

    emprestar(usuario, livro){
        const livro_existe = this.#livros.some((l) => l.isbn == livro.isbn)
        if(!livro_existe){
            console.log("Livro não encontrado")
            return false
        }

        if (livro.status != "disponivel"){
            console.log("Livro não disponivel no momento")
            return false
        }

        if (usuario.quantidade_emprestados == usuario.limite){
            console.log("Limite de emprestimos atingido pelo usuario")
            return false
        }

        usuario.emprestar(1)

        livro.status = "emprestado"

        const novo_emprestimo = new Emprestimo(usuario, livro)

        usuario.historico.adicionar_emprestimo(novo_emprestimo)

        console.log("tudo certo")
        
    }
}

class Historico{
    #historico

    constructor(){
        this.#historico = []
    }

    adicionar_emprestimo(emprestimo){
        this.#historico.push(emprestimo)
    }

    listar_historico(){
        console.log("\nHistorico de livros\n")
        if(this.#historico.length == 0){
            console.log("Não há registros")
            return
        }

        for (const i of this.#historico){
            console.log(`Usuario: ${i.usuario.nome}\nLivro: ${i.livro.titulo}\n`)
        }
        
    }


}

class Emprestimo{
    #usuario
    #livro

    constructor(usuario, livro){
        this.#usuario = usuario
        this.#livro = livro
    }

    get usuario(){
        return this.#usuario

    }

    get livro(){
        return this.#livro
    }
}


const isa = new Aluno("isa", "111", "2222")
// isa.emprestar(2)
// isa.devolver(1)
// isa.emprestar(3)

const livro1 = new Livro("111", "ai", "oi")
// livro1.status = "emprestado"

const biblioteca = new Biblioteca()
biblioteca.cadastrar_livro(livro1)

biblioteca.emprestar(isa, livro1)

isa.historico.listar_historico()
.....

class BancoCentral{
    static transferir(emitente, destino, valor, senha){
        if(emitente.sacar(valor, senha)){
            destino.depositar_via_BC(valor)
        }
        console.log("deposito bc concluido")

    }
}

class Historico{
    #historicos

    constructor(){
        this.#historicos = []
    }

    adicionar(transacao){
        this.#historicos.push(transacao)
    }

    listar(){

        for (const i of this.#historicos){
            console.log(`\nTipo: ${i.tipo}\nValor: ${i.valor}`)
        }


    }
}

class Transacao{
    #tipo
    #valor

    constructor(tipo, valor){
        this.#tipo = tipo
        this.#valor = valor
    }

    get tipo(){
        return this.#tipo
    }

    get valor(){
        return this.#valor
    }

}

class Pessoa{
    #nome
    #cpf

    constructor(nome, cpf){
        this.#nome = nome
        this.#cpf = cpf
    }

    criar_conta(){
        return new Conta(this)
    }

    get nome(){
        return this.#nome
    }
}

class Conta{
    #senha = "1234"
    #dono
    #saldo
    #historico

    constructor(dono){
        this.#dono = dono
        this.#saldo = 0
        this.#historico = new Historico()
    }

    extrato(senha){
        if (!this.verificar_senha(senha)){
            console.log("\nSenha incorreta!")
            return
        }

        console.log("\nConta: "+this.#dono.nome + "\nSaldo em conta: \nR$ "+ this.#saldo)

    }

    sacar(valor, senha){
        if (!this.verificar_senha(senha)){
            console.log("\nSenha incorreta!")
            return false
        }

        if (valor > this.#saldo){
            console.log("\nSaldo insuficiente!")
            return false
        }

        this.#saldo -= valor
        console.log("\nSaque realizado com sucesso!")
        // console.log(this.#saldo)

        const transacao = new Transacao("SAQUE", valor)
        this.#historico.adicionar(transacao)


        return true
       

    }

    depositar(valor, senha){
        if (!this.verificar_senha(senha)){
            console.log("\nSenha incorreta!")
            return
        }
        if (valor < 0){
            console.log("\nDeposito negado!")
            return
        }

        if (valor > 2000){
            console.log("\nLimite de deposito excedido!")
            return 
        }
        this.#saldo += valor

        const transacao = new Transacao("DEPOSITO", valor)

        this.#historico.adicionar(transacao)

        // console.log(this.#historico.listar())
        console.log("\nDeposito realizado com sucesso!")
        // console.log(this.#saldo)

    }

    depositar_via_BC(valor){
        this.#saldo += valor
        console.log("deposito via bc, saldo: "+this.#saldo)

        const transacao = new Transacao("DEPOSITO BC", valor)
        this.#historico.adicionar(transacao)

    }

    verificar_senha(senha){
        return senha == this.#senha ? true : false
    }

    mostrar_valor(){
        console.log("\nHistórico da conta: "+this.#dono.nome)

        this.#historico.listar()
    }
}

isa = new Pessoa("isa", "1234")
isaConta = isa.criar_conta()

isaConta.depositar(100, "1234")
// isaConta.sacar(60, "1234")
isaConta.extrato("1234")

belli = new Pessoa("belli", "1342")
belliConta = belli.criar_conta()

belliConta.extrato("1234")

BancoCentral.transferir(isaConta, belliConta, 20, "1234")

isaConta.extrato("1234")
belliConta.extrato("1234")

isaConta.mostrar_valor()
belliConta.mostrar_valor()
.....

class BancoCentral{
     static transferirValor(emitente, destino, valor, senha)
     {        
          if(emitente.sacar(valor,senha))
            destino.depositoViaBC(valor);
     }
}

class Pessoa{
    #nome
    #cpf
    #dataNasc
    #endereco
    
    get nome()
    {
        return this.#nome;
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

    //get sempre retorna algo
    // get saldo()    { 
    //     if( this.#saldo <= 300 )
    //          return this.#saldo;
    //     else
    //         return "Valor impede demonstração";
    // }

    //set é pra atualizar o valor
    // set saldo(valor)    {
    //     this.#saldo += valor;
    // }

    sacar(valorSaque, senha) {    
        if(valorSaque <= 0)
        {
            console.log("Não é possível sacar valor negativo")
            return false
        }    
        if(!this.validarSenha(senha))
        {
            console.log("Senha incorreta")
            return false
        }
        if ((this.#saldo - valorSaque) >= 0) {
            this.#saldo -= valorSaque
            return true
        }else
            {
                console.log("Saldo insuficiente")
                return false
            }

    }

    extrato(senha)
    {
        if(!this.validarSenha(senha))
        {
            console.log("Senha incorreta")
            return
        }
        console.log(`${this.#dono.nome}, seu saldo é ${this.#saldo}`)
    }

    depositar(valor,senha)//sobrecarga
    {
        if(!this.validarSenha(senha))
        {
            console.log("Senha incorreta")
            return
        }
        if(valor < 0){
            console.log("O valor depositado não pode ser negativo")
            return;
        }
        if(valor > 5000){
            console.log("Valor muito alto. Dirija-se ao caixa. Atendimento das 10:00h às 15:00hs")
            return;
        }
        this.#saldo += valor
    }

    depositoViaBC(saldo)
    {
        this.#saldo += saldo
    }

    validarSenha(senha)
    {
        return senha === this.#senha
    }
}

Jose = new Pessoa(126365, "José Silva", "10/10/88")
Manoel = new Pessoa(123654,"Manoel Costa", "10/10/10")

contaJose = Jose.criarConta()
contaManoel = Manoel.criarConta()
contaCopia = contaJose
//Fazer operação de transferência
contaJose.depositar(50, "admin")
contaManoel.depositar(150, "admin")
contaJose.extrato("admin")
contaManoel.extrato("admin")

// BC = new BancoCentral()
BancoCentral.transferirValor(contaJose, contaManoel, 40, "admin")

contaJose.extrato("admin")
contaManoel.extrato("admin")
contaCopia.extrato("admin")
contaCopia2 = contaCopia

contaCopia.depositar(1000,"admin")
console.log("---------------------------")
contaJose.extrato("admin")
contaManoel.extrato("admin")
contaCopia.extrato("admin")
console.log("--")
contaCopia2.extrato("admin")


....

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
contaCopia2.extrato("admin");


......

class Conta{
    #dono
    #saldo
    #senha

    //  # funciona como um private que não deixa o usuario atribui valor direto, só por meio de metodos

    constructor(dono){
        this.#saldo = 200;
        this.#senha = '1234'
        this.#dono = dono
    }


    // get sempre retorna algo
    // set sempre atualiza o valor
    // torna a variavel privada em publica com controle

    get saldo()
    {
        if (this.#saldo <= 3)
            return this.#saldo;
        else
            return "valor impede demonstração";
    }

    set saldo(valor)
    {
        this.#saldo += valor;
    }

    sacar(valorSaque, senha){
        if((this.#saldo - valorSaque) >= 0 && this.#senha == senha){
            this.#saldo -= valorSaque
        }else{
            console.log("Saldo insuficiente")
            return
        }

    }

    extrato(senha){
        if(this.#senha == senha){

            console.log(this.#saldo)
        }
    }

    depositar(valor, senha){
        // if(this.#senha != senha){
        //     console.log("senha incorreta")
        //     return;
        // }
        if(!this.validar_senha(senha)){ //chamando o metodo
            console.log("senha incorreta")
        //     return;
        }
        if(valor < 0){
            console.log("Não é possivel fazer um deposito negativo")
            return;
        }
        if(valor > 5000){
            console.log("Não é possivel fazer um deposito maior que 5000")
            return;
        }
        this.#saldo += valor
        
    }

    // add_dono(dono, senha){
    //     if(!this.validar_senha(senha)){
    //         return "Senha incorreta"
    //     }else{
    //         this.dono = dono
    //     }

    // }

    validar_senha(senha){
        return senha == this.#senha //vai retornar um bool
    }



}

// minhaconta = new Conta()
// minhaconta.#saldo += 200
// minhaconta.sacar(50, "1234")
// minhaconta.sacar(250)
// console.log(minhaconta) 
// minhaconta.extrato()
// minhaconta.depositar(700)
// minhaconta.extrato('1234')

class Pessoa{
    nome
    cpf
    telefone
    #idconta

    constructor(nome,cpf, telefone){
        this.nome = nome
        this.cpf = cpf
        this.telefone = telefone
    }
    // colocar um metodo que cria uma nova conta aqui dentro

}

pessoa1 = new Pessoa("isa", "12312312312", "12121212")
// console.log(pessoa1)
// minhaconta.add_dono(pessoa1, '1234')
// console.log(minhaconta)
conta_pessoa1 = new Conta(pessoa1)

console.log(pessoa1)
console.log(conta_pessoa1)


......





