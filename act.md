# Act (agir)

É um método da lib Testing Library (React) usado para envolver uma AÇÃO que pode causar uma atualização no estado do React durante o teste.

Ele informa ao React que estamos realizando uma ação que pode provocar atualizações, permitindo que essas atualizações sejam processadas antes de continuarmos com o teste.

É útil principalmente quando estamos testando:

alteração de estado;
chamadas de funções que atualizam o estado;
efeitos (useEffect);
eventos;
atualizações assíncronas.

---

# Sintaxe - Act 

```ts
await act(async () => {
    result.current.fazerAlgumaCoisa();
});
```

`await`
- faz o código esperar a conclusão de uma Promise antes de continuar.

`act()`
- envolve a ação que pode provocar atualizações no React;
- cria um contexto em que o React consegue processar essas atualizações antes que o teste continue.

`async`
- indica que a função passada para o act é assíncrona;
permite utilizar await dentro dessa função.

`()`
- indicam os argumentos que estamos passando para o act;
nesse caso, estamos passando uma função assíncrona;
- essa função acessa o objeto result, gerado pelo renderHook, para executar uma ação no Hook que estamos testando de maneira isolada.

`result.current`
- acessa o resultado atual do Hook através do objeto result, gerado pelo renderHook.

`fazerAlgumaCoisa`
- é o método retornado pelo Hook que queremos executar;
- geralmente pode ser um método responsável por alterar algum estado, como um setAlgumaCoisa;
- quando esse método provoca uma atualização de estado, colocamos sua execução dentro do act.

----

# Quando usar o Act?

Usamos o act quando uma ação realizada durante o teste pode provocar uma atualização no React e precisamos garantir que essa atualização seja processada antes de fazer a verificação.

Por exemplo:

```ts
await act(async () => {
    result.current.incrementar();
});
expect(result.current.count).toBe(1);
```

Nesse exemplo:

act envolve a chamada do método incrementar();
incrementar() provoca uma atualização;
depois que o act termina, fazemos a verificação com expect;
result.current.count é usado para verificar o estado atual.


----

# Act sincrono X assincrono

### Act síncrono
É utilizado quando a ação dentro do act é síncrona.
```ts
act(() => {
    result.current.incrementar();
});
```
Nesse caso:
- não usamos async;
- não usamos await;
- a função executada dentro do act é síncrona.

### Act assíncrono
É utilizado quando a ação dentro do act é assíncrona.

```ts
await act(async () => {
    await result.current.buscarDados();
});
```

Nesse caso:
- async indica que a função é assíncrona;
- await dentro da função espera a operação assíncrona;
- await antes do act espera a conclusão do act antes de continuar o teste.