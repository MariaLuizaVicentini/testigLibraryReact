# waitFor
É um método da lib Testing Library usado para aguardar uma condição ser satisfeita antes de continuar o teste.

É útil principalmente quando o comportamento que queremos testar não acontece imediatamente, por exemplo:
- uma atualização de estado;
- uma requisição assíncrona;
- um efeito (useEffect);
- uma mudança no DOM;
- uma função que é chamada depois de uma operação assíncrona.

---

## Sintaxe - waitFor

```ts
await waitFor(() => {
    expect(...);
});
```

`await`
- Indica que o teste deve aguardar o resultado do waitFor 

`waitFor`
- É o método da Testing Library responsável por aguardar até que uma determinada condição seja satisfeita


## Quando usar o waitFor?

Usamos o waitFor quando queremos aguardar o resultado de um comportamento assíncrono antes de fazer uma asserção.

Por exemplo, se temos uma função que realiza uma operação assíncrona e, depois dela, uma função mockada deve ser chamada:

```ts
await waitFor(() => {
    expect(mockFunction).toHaveBeenCalled();
});
```
