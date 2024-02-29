interface AccountState {
    void deposit(double amount, Account account);
    void withdraw(double amount, Account account);
    void activate(Account account);
    void suspend(Account account);
    void close(Account account);
}

class ActiveState implements AccountState {
    @Override
    public void deposit(double amount, Account account) {
        account.setBalance(account.getBalance() + amount);
        System.out.println("Deposit of " + amount + " successful. Current balance: " + account.getBalance());
    }

    @Override
    public void withdraw(double amount, Account account) {
        if (account.getBalance() >= amount) {
            account.setBalance(account.getBalance() - amount);
            System.out.println("Withdrawal of " + amount + " successful. Current balance: " + account.getBalance());
        } else {
            System.out.println("Insufficient funds. Withdrawal failed.");
        }
    }

    @Override
    public void activate(Account account) {
        System.out.println("Account is already activated!");
    }

    @Override
    public void suspend(Account account) {
        account.setAccountState(new SuspendedState());
        System.out.println("Account is suspended!");
    }

    @Override
    public void close(Account account) {
        account.setAccountState(new ClosedState());
        System.out.println("Account is closed!");
    }
}

class SuspendedState implements AccountState {
    @Override
    public void deposit(double amount, Account account) {
        System.out.println("You cannot deposit in a suspended account.");
    }

    @Override
    public void withdraw(double amount, Account account) {
        System.out.println("You cannot withdraw from a suspended account.");
    }

    @Override
    public void activate(Account account) {
        account.setAccountState(new ActiveState());
        System.out.println("Account is activated!");
    }

    @Override
    public void suspend(Account account) {
        System.out.println("Account is already suspended!");
    }

    @Override
    public void close(Account account) {
        account.setAccountState(new ClosedState());
        System.out.println("Account is closed!");
    }
}

class ClosedState implements AccountState {
    @Override
    public void deposit(double amount, Account account) {
        System.out.println("You cannot deposit in a closed account.");
    }

    @Override
    public void withdraw(double amount, Account account) {
        System.out.println("You cannot withdraw from a closed account.");
    }

    @Override
    public void activate(Account account) {
        System.out.println("You cannot activate a closed account!");
    }

    @Override
    public void suspend(Account account) {
        System.out.println("You cannot suspend a closed account!");
    }

    @Override
    public void close(Account account) {
        System.out.println("Account is already closed!");
    }
}

class Account {
    private String accountNumber;
    private double balance;
    private AccountState accountState;

    public Account(String accountNumber, double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
        this.accountState = new ActiveState();
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public void setAccountNumber(String accountNumber) {
        this.accountNumber = accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public AccountState getAccountState() {
        return accountState;
    }

    public void setAccountState(AccountState accountState) {
        this.accountState = accountState;
    }

    public void deposit(double amount) {
        accountState.deposit(amount, this);
        System.out.println(toString());
    }

    public void withdraw(double amount) {
        accountState.withdraw(amount, this);
        System.out.println(toString());
    }

    public void activate() {
        accountState.activate(this);
        System.out.println(toString());
    }

    public void suspend() {
        accountState.suspend(this);
        System.out.println(toString());
    }

    public void close() {
        accountState.close(this);
        System.out.println(toString());
    }

    @Override
    public String toString() {
        return "Account{" +
                "accountNumber='" + accountNumber + '\'' +
                ", balance=" + balance +
                '}';
    }
}

public class Main {
    public static void main(String[] args) {
        Account myAccount = new Account("1234", 10000.0);

        myAccount.activate();
        System.out.println();

        myAccount.suspend();
        System.out.println();

        myAccount.activate();
        System.out.println();

        myAccount.deposit(10000.0);
        System.out.println();

        myAccount.withdraw(100.0);
        System.out.println();

        myAccount.close();
        System.out.println();

        myAccount.activate();
        System.out.println();

        myAccount.close();
        System.out.println();

        myAccount.activate();
        System.out.println();

        myAccount.suspend();
        System.out.println();

        myAccount.withdraw(500.0);
        System.out.println();

        myAccount.deposit(1000.0);
    }
}
