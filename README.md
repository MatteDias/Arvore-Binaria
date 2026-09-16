public class ArvoreBinaria {

    class No {
        int valor;
        No esq;
        No dir;

        No(int valor) {
            this.valor = valor;
            esq = null;
            dir = null;
        }
    }

    No raiz = null;

    public void inserir(int valor) {
        if (raiz == null) {
            raiz = new No(valor);
            return;
        }

        No atual = raiz;
        while (true) {
            if (valor < atual.valor) {
                if (atual.esq == null) {
                    atual.esq = new No(valor);
                    return;
                }
                atual = atual.esq;
            } else {
                if (atual.dir == null) {
                    atual.dir = new No(valor);
                    return;
                }
                atual = atual.dir;
            }
        }
    }

    public boolean buscar(int valor) {
        No atual = raiz;

        while (atual != null) {
            if (atual.valor == valor) {
                return true;
            }

            if (valor < atual.valor) {
                atual = atual.esq;
            } else {
                atual = atual.dir;
            }
        }

        return false; 
    }

    public boolean remover(int valor) {
        if (buscar(valor) == false) {
            return false;
        }

        raiz = removeAux(raiz, valor);
        return true;
    }

    private No removeAux(No no, int valor) {
        if (no == null) {
            return no;
        }

        if (valor < no.valor) {
            no.esq = removeAux(no.esq, valor);
        } else if (valor > no.valor) {
            no.dir = removeAux(no.dir, valor);
        } else {

            if (no.esq == null && no.dir == null) {
                return null; 
            }

            if (no.esq == null) {
                return no.dir;
            }

            if (no.dir == null) {
                return no.esq;
            }

            No menorDaDireita = no.dir;
            while (menorDaDireita.esq != null) {
                menorDaDireita = menorDaDireita.esq;
            }

            no.valor = menorDaDireita.valor;
            no.dir = removeAux(no.dir, menorDaDireita.valor);
        }

        return no;
    }

    public void printInOrder() {
        printInOrderAux(raiz);
        System.out.println();
    }

    private void printInOrderAux(No no) {
        if (no == null) return;

        printInOrderAux(no.esq);
        System.out.print(no.valor + " ");
        printInOrderAux(no.dir);
    }

    public static void main(String[] args) {
        ArvoreBinaria arvore = new ArvoreBinaria();

        int[] valores = {50, 30, 70, 20, 40, 60, 80, 40, 70};
        for (int i = 0; i < valores.length; i++) {
            arvore.inserir(valores[i]);
        }

        System.out.print("arvore: ");
        arvore.printInOrder();

        System.out.println("tem o 40? " + arvore.buscar(40));
        System.out.println("tem o 100? " + arvore.buscar(100));

        System.out.println("removendo o 30...");
        arvore.remover(30);
        arvore.printInOrder();

        System.out.println("removendo o 999 (nao existe): " + arvore.remover(999));

        System.out.println("removendo o 80...");
        arvore.remover(80);
        arvore.printInOrder();
    }
}
