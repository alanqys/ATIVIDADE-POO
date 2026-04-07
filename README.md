class Conta {
    private double saldo;

    public double getSaldo() {
        return this.saldo;
    }

    protected void setSaldo(double saldo) {
        this.saldo = saldo;
    }

    public void deposita(double valor) {
        if (valor > 0) {
            this.saldo += valor;
        }
    }

    public void atualiza(double taxaSelic) {
        this.saldo = this.saldo * (1 + taxaSelic);
    }
}

class ContaCorrente extends Conta {
    @Override
    public void atualiza(double taxaSelic) {
        double novoSaldo = this.getSaldo() * (1 + (taxaSelic * 2)) - 15;
        this.setSaldo(novoSaldo);
    }
}

class ContaPoupanca extends Conta {
    @Override
    public void atualiza(double taxaSelic) {
        double novoSaldo = this.getSaldo() * (1 + (taxaSelic * 0.75));
        this.setSaldo(novoSaldo);
    }
}

class AtualizadorDeContas {
    private double saldoTotal = 0;
    private double selic;

    AtualizadorDeContas(double selic) {
        this.selic = selic;
    }

    public void roda(Conta c) {
        System.out.println("Saldo anterior: " + c.getSaldo());
        c.atualiza(this.selic);
        System.out.println("Saldo novo: " + c.getSaldo());
        this.saldoTotal += c.getSaldo();
    }

    public double getSaldoTotal() {
        return this.saldoTotal;
    }
}

public class Main {
    public static void main(String[] args) {
        Conta c = new Conta();
        ContaCorrente cc = new ContaCorrente();
        ContaPoupanca cp = new ContaPoupanca();

        c.deposita(1000);
        cc.deposita(1000);
        cp.deposita(1000);

        AtualizadorDeContas adc = new AtualizadorDeContas(0.08);

        adc.roda(c);
        adc.roda(cc);
        adc.roda(cp);

        System.out.println("Saldo Total Acumulado: " + adc.getSaldoTotal());
    }
}
