# RenderHook

É um método da lib Testing Library (React) que serve para testar **Hooks customizados isoladamente**, sem precisar criar um componente React só para poder chamá-los.

No React, não conseguimos chamar um Hook fora de um componente. O `renderHook` resolve isso criando um **componente "fantasma" por baixo dos panos** para executar o nosso Hook customizado e capturar o retorno dele.

---

## Sintaxe - RenderHook

```ts
const { result, rerender, unmount } = renderHook(
    (propsIniciais) => usarSeuHook(propsIniciais),
    { initialProps: { valor: 10 }}
);
```

`const  { }`
- indicam que estamos fazendo uma desestruturaçao do objeto retornado pelo renderHook

O `renderHook` aceita dois params principais:

* **function** que executa o Hook, onde chamamos o Hook que desejamos testar
* **objeto de configuração** (opcional)

  * `initialProps`: objeto com os valores ou propriedades iniciais que serão passados para o Hook
  * `wrapper`: um componente React que envolve o Hook durante o teste

`(propsIniciais) => usarSeuHook(propsIniciais)`

* Função de callback
* É executada pelo `renderHook`
* É dentro dela que chamamos o Hook que queremos testar
* O valor retornado pelo Hook fica disponível através do `result`

`{ initialProps: { valor: 10 } }`

* `initialProps` define as props iniciais que serão enviadas para a função de callback
* Nesse exemplo, `{ valor: 10 }` será recebido pelo parâmetro `propsIniciais`

```ts
(propsIniciais) => usarSeuHook(propsIniciais)
```

### result

Contém o resultado retornado pelo Hook.

Para acessar o valor retornado pelo Hook, usamos:

```ts
result.current
```

Exemplo:

```ts
const { result } = renderHook(
    () => usarSeuHook()
);

expect(result.current).toBe(...);
```

### rerender

Permite executar novamente o Hook, simulando uma nova renderização do componente.

É útil principalmente quando o Hook recebe props e queremos testar seu comportamento quando essas props mudam.

```ts
const { result, rerender } = renderHook(
    (props) => usarSeuHook(props),
    { initialProps: { valor: 10 }}
);

rerender({ valor: 20 });
```

Nesse caso:

```text
1. Hook é executado com valor = 10
2. rerender() é chamado
3. Hook é executado novamente com valor = 20
```

### unmount

Desmonta o componente "fantasma" criado pelo `renderHook`.

É útil para testar comportamentos que acontecem quando o componente é desmontado, como o `cleanup` de um `useEffect`.

```ts
const { unmount } = renderHook(
    () => usarSeuHook()
);

unmount();
```

---

## Quando usar o RenderHook?

Usamos o `renderHook` quando queremos testar o **comportamento de um Hook customizado diretamente**, sem precisar testar um componente que utiliza esse Hook.

Por exemplo, se temos:

```ts
const useCounter = () => {
    const [count, setCount] = useState(0);

    return { count, setCount };
};
```

Podemos testar o Hook diretamente:

```ts
const { result } = renderHook(() => useCounter());

expect(result.current.count).toBe(0);
```

Assim, o foco do teste fica no comportamento do Hook, e não na renderização de um componente.
