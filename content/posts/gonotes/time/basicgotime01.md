---
title: "Basic about Go Time"
date: 2023-08-14T13:37:35-03:00
author: "Fernandoi L. Couto"
---
### Estrutura do Pacote 


O pacote **Time** contem um poderoso ferramental que permite ao utilizador de _Go_ manipular/exibir[^1] dados sobre o _tempo._  
Recursos disponibilizados:

[Funções:](https://pkg.go.dev/time@go1.21.0#pkg-functions)
1. After()
2. Sleep()
3. Tick()

[Types:](https://pkg.go.dev/time@go1.21.0#pkg-types)
1. Duration
2. Location
3. Month
4. ParseError
5. Ticker
6. Time
7. Timer
8. Weekday

Além das três funções elencadas, cada _Type_ possui seu conjunto de _Métodos_ que entregam versatilidade no manuseio 
de operações envolvendo _tempo_. Esses recursos estão distribuidos nos [arquivos fonte](https://pkg.go.dev/time@go1.21.0#section-sourcefiles): 

1. format.go
2. format_rfc3339.go
3. sleep.go
4. sys_unix.go
5. tick.go
6. time.go
7. zoneinfo.go
8. zoneinfo_goroot.go
9. zoneinfo_read.go
10. zoneinfo_unix.go

Disponível em: [https://cs.opensource.google/go/go/+/go1.21.0:src/time](https://cs.opensource.google/go/go/+/go1.21.0:src/time)


### Datas e Faixas de Datas


O Pacote _Time_ utiliza um conjunto de [Constantes](https://pkg.go.dev/time@go1.21.0#pkg-constants) que auxiliam no manuseio e  
e formatação de objetos e operações usando _tempo_. Para exemplificar, e sem nenhuma vontade de esgotar o assunto, a constante 
_Layout_ será discutida a seguir.

```Go
const Layout = "01/02 03:04:05PM '06 -0700" // The reference time, in numerical order.
objDate := time.Date(2023, 8, 15, 14, 57, 30, 0, time.Local)
fmt.Printf("objDate: %v\n", objDate.Format(Layout))

// Output:
objDate: 08/15 02:57:30PM '23 -0300
```

Essa referência do tempo em ordem numérica indica o _output_ escolhido. O exemplo acima usa uma sugestão contida na documentação 
e o examplo a seguir tem uma adaptação para o Brasil.

```Go
const Layout02 = "02/01/2006 15:04:05 -0700"
objDate := time.Date(2023, 8, 15, 14, 57, 30, 0, time.Local)
fmt.Printf("objDate: %v\n", objDate.Format(Layout02))

// Output:
objDate: 15/08/2023 14:57:30 -0300

// Importante: 

Sobre os 7 (sete) parâmetros usados em **const Layout02 = "02/01/2006 15:04:05 -0700"**, 
tem-se:

1. **02** dia
2. **01** mês
3. **2006** ou **06** ano
4. **03** ou **15** hora
5. **04** minuto
6. **05** segundo
7. **-07** ou **-0700** time.UTC, time.Local

```

### Type time.Time 


_time.Time_ é um objeto do tipo [struct](https://go.dev/ref/spec#Struct_types) e tem como _valor zero_ 
**January 1, year 1, 00:00:00.000000000 UTC**. Esse valor pode ser detectado pela função **IsZero()**.




[^1]: "Os cálculos do calendário sempre assumem um calendário gregoriano, sem segundos bissextos"


