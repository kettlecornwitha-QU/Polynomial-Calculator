from IPython import get_ipython
from IPython.display import Math, display
from sympy import (Symbol, Matrix, latex, Eq, Expr, sympify)


def format_num(num: float) -> int or float:
    num = round(num, 5)
    if num == int(num):
        num = int(num)
    return num

    
def is_notebook() -> bool:
    if get_ipython is not None:
        return True


class Sys:
    def __init__(self, ins: list, outs: list) -> None:
        self.n = len(ins) - 1
        self.ins = ins
        self.outs = outs
        self.coefs = self.calculate_coefs()

    @staticmethod
    def get_degree() -> int:
        while True:
            n = input('Enter the polynomial degree:  ')
            if n.isnumeric():
                return int(n)
            print('Degree needs to be a positive integer!')

    @staticmethod
    def get_inputs(deg: int) -> list:
        print(f'\nEnter {deg+1} input values with known output:')
        ins = []
        for v in range(deg+1):
            while True:
                t = input()
                try:
                    float(t)
                except:
                    print('Input must be an integer or decimal!')
                    continue
                ins.append(float(t))
                break
        return ins

    @staticmethod
    def get_outputs(ins: list) -> list:
        outs = []
        for inp in ins:
            while True:
                t = input(f'What is f({format_num(inp)})?')
                try:
                    float(t)
                except:
                    print('Output must be an integer or decimal!')
                    continue
                outs.append(float(t))
                break
        return outs

    def calculate_coefs(self) -> list:
        M = Matrix([[inp**i for i in range(self.n+1)] for inp in self.ins])
        M = M.col_insert(self.n+1, Matrix(self.outs))
        m_coefs = M.rref(pivots=False).col(self.n+1).tolist()
        return list(reversed([format_num(coef[0]) for coef in m_coefs]))

    def poly_expr(self) -> Expr:
        expr = sympify(f'{self.coefs[0]}*x^{self.n}')
        for i, coef in enumerate(self.coefs[1:]):
            term = sympify(f'{coef}*x^{self.n-i-1}')
            expr = expr + term
        return expr

    def print_poly(self) -> None:
        raw_latex = latex(Eq(Symbol('f(x)'), self.poly_expr()))
        if is_notebook():
            display(Math(raw_latex))
        else:
            print(raw_latex)

    @classmethod
    def from_stdin(cls) -> 'Sys':
        ins = cls.get_inputs(cls.get_degree())
        outs = cls.get_outputs(ins)
        return cls(ins, outs)

    @classmethod
    def from_demo(cls) -> 'Sys':
        ins = [0, 1, 4, 5.5, 3]
        out = [1, 1, 0, 29, 3.1]
        return cls(ins, out)


if __name__ == '__main__':
    #Sys = Sys.from_demo()
    Sys = Sys.from_stdin()
    Sys.print_poly()
