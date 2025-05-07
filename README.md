# O Oscar

# Nomeados ao Oscar - Atualizado

Contém a base de indicados ao Oscar em formato noSQL para treinar comandos CRUD.

Abaixo, algumas atividades para trabalharmos.

---

- Atualize os registros da tabela com os dados do Oscar 2025

  **Os registros foram atualizados com as seguintes categorias:**

1. Best Picture;
2. Best Actor (Actor in a Leading Role)
3. Best Supporting Actor (Actor in a Supporting Role)
4. Best Supporting Actress (Actress in a Supporting Role)
5. Best Animated Feature Film ( Animated Feature Film)
6. Best Documentary Feature (Documentary Feature Film)
7. Best Costume Design (Costume Design)

---

- Qual o **total** de registros na tabela indicados?

 R: Após atualizações, temos 10929 documentos

 Q:

```
db.oscar.countDocuments()
< 10929
```

---

- Qual o número de indicações por categoria agrupadas por categoria?

R: 2

Q:

```
db.oscar.aggregate([
  {
    $group: {
      _id: "$categoria",
      total_indicacoes: { $sum: 1 }
    }
  },
  {
    $sort: { total_indicacoes: -1 }
  }
]);

```

---

- Quantas vezes Natalie Portman foi indicada ao Oscar?

R: 3 vezes

Q:

```
 db.oscar.find({"Nome": "Natalie Portman"}).countDocuments()

```

---

- Quantos Oscars Natalie Portman ganhou?

 R: Natalie Portman ganhou apenas uma vez, com o filme “Black Swan”.

 Q: 

```
db.oscar.find({nome_do_indicado: "Natalie Portman", vencedor: "true"})

{
  "_id": {
    "$oid": "6818b0f76a44c95738f4d7d5"
  },
  "id_registro": 9156,
  "ano_filmagem": 2010,
  "ano_cerimonia": 2011,
  "cerimonia": 83,
  "categoria": "ACTRESS IN A LEADING ROLE",
  "nome_do_indicado": "Natalie Portman",
  "nome_do_filme": "Black Swan",
  "vencedor": "true"
}
```

---

- Quantas vezes Viola Davis foi indicada ao Oscar?

 R: 4 vezes

 Q:

```
 db.oscar.find({"Nome": "Viola Davis"}).countDocuments()

```

---

- Quantos Oscars Viola Davis ganhou?

 R: uma vez

 Q: 

```
db.oscar.find({nome_do_indicado: "Viola Davis", vencedor: "true"})

{
  "_id": {
    "$oid": "6818b0f76a44c95738f4dac2"
  },
  "id_registro": 9905,
  "ano_filmagem": 2016,
  "ano_cerimonia": 2017,
  "cerimonia": 89,
  "categoria": "ACTRESS IN A SUPPORTING ROLE",
  "nome_do_indicado": "Viola Davis",
  "nome_do_filme": "Fences",
  "vencedor": "true"
}
```

---

- Amy Adams já ganhou algum Oscar?

 R: Não, apesar de ter sido indicada **seis vezes** 

 Q: (Não houve correspondências com o campo vencedor: “true” )

```
db.oscar.find({nome_do_indicado: "Amy Adams", vencedor: "true"})
```

---

- Quais os atores/atrizes que foram indicados mais de uma vez

  R: 

 Q: 

```
db.nome_da_sua_collection.aggregate([
  {
    $group: {
      _id: "$nome_do_indicado",
      totalIndicacoes: { $sum: 1 }
    }
  },
  {
    $match: {
      totalIndicacoes: { $gt: 1 }
    }
  },
  {
    $sort: {
      totalIndicacoes: -1 }
  }
])

```

---

- A série de filmes Toy Story ganhou Oscars em quais anos?

 R: "*Toy Story (1995) não venceu categorias competitivas, mas recebeu um Oscar honorário em 1996. Já os filmes da série Toy Story ganharam Oscars em edições posteriores, especialmente o 3 e o 4.*"

Se formos procurar no database sobre o histórico aparece o seguinte:

 Q:

```
db.oscar.find({nome_do_filme: "Toy Story"})

{
  "_id": {
    "$oid": "6818b0f76a44c95738f4d18e"
  },
  "id_registro": 7549,
  "ano_filmagem": 1995,
  "ano_cerimonia": 1996,
  "cerimonia": 68,
  "categoria": "WRITING (Screenplay Written Directly for the Screen)",
  "nome_do_indicado": "Screenplay by Joss Whedon, Andrew Stanton, Joel Cohen, Alec Sokolow;  Story by John Lasseter, Peter Docter, Andrew Stanton, Joe Ranft",
  "nome_do_filme": "Toy Story",
  "vencedor": 0
}

{
  "_id": {
    "$oid": "6818b0f76a44c95738f4d166"
  },
  "id_registro": 7509,
  "ano_filmagem": 1995,
  "ano_cerimonia": 1996,
  "cerimonia": 68,
  "categoria": "MUSIC (Original Musical or Comedy Score)",
  "nome_do_indicado": "Randy Newman",
  "nome_do_filme": "Toy Story",
  "vencedor": 0
}

{
  "_id": {
    "$oid": "6818b0f76a44c95738f4d16c"
  },
  "id_registro": 7515,
  "ano_filmagem": 1995,
  "ano_cerimonia": 1996,
  "cerimonia": 68,
  "categoria": "MUSIC (Original Song)",
  "nome_do_indicado": "Music and Lyric by Randy Newman",
  "nome_do_filme": "Toy Story",
  "vencedor": 0
}
```

Ou seja, há apenas o registro da concorrência do primeiro filme, e os demais não estão no database.

---

- A partir de que ano que a categoria "Actress" deixa de existir?

 R:  Há correspondência a categoria “Actress” na coleção, porém existe a categoria  “Best Actress in a Leading Role”, a qual, nunca deixou de existir. São apenas jeitos diferentes de nomear a mesma categoria. 😀

---

- Quem ganhou o primeiro Oscar para Melhor Atriz? Em que ano?

 R: Janet Gaynor. Ganhadora em 1929.

Q: 

```
db.oscar.find(
  { categoria: /actress/i, vencedor: 1 }
).sort({ cerimonia: 1 }).limit(1)

{
  _id: ObjectId('6818b0f56a44c95738f4b415'),
  id_registro: 4,
  ano_filmagem: 1927,
  ano_cerimonia: 1928,
  cerimonia: 1,
  categoria: 'ACTRESS',
  nome_do_indicado: 'Janet Gaynor',
  nome_do_filme: '7th Heaven',
  vencedor: 1
}
```

---

- Na campo "Vencedor", altere todos os valores com "true" para 1 e todos os valores "false" para 0.

Q:

```
db.oscar.updateMany(
  { vencedor: { $in: ["true", "false"] } },
  [
    {
      $set: {
        vencedor: {
          $cond: { if: { $eq: ["$vencedor", "true"] }, then: 1, else: 0 }
        }
      }
    }
  ]
)

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 10927,
  modifiedCount: 10927,
  upsertedCount: 0
}
```

 

---

- Em qual edição do Oscar "Crash" concorreu ao Oscar?

 R: O filme “Crash”, concorreu ao Oscar na categoria ‘Best Picture” no ano de 2006.

 Q: 

```
db.oscar.find({categoria: "BEST PICTURE", nome_do_filme: "Crash"})

{
  "_id": {
    "$oid": "6818b0f76a44c95738f4d5d3"
  },
  "id_registro": 8642,
  "ano_filmagem": 2005,
  "ano_cerimonia": 2006,
  "cerimonia": 78,
  "categoria": "BEST PICTURE",
  "nome_do_indicado": "Paul Haggis and Cathy Schulman, Producers",
  "nome_do_filme": "Crash",
  "vencedor": 1
}
```

---

- O filme Central do Brasil aparece no Oscar?

  R: Não aparece nenhum documento correspondente no Database. (Porém sim, já concorreu a categorias no Oscar).

---

- Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser.

Q:

```
db.oscar.insertMany([
  {
    "_id": { "$oid": "5fcae4a2e9b6b2e8c7dba1c1" },
    "id_registro": 1,
    "nome_do_filme": "Blade Runner 2049",
    "ano_filmagem": 2017,
    "direcao": "Denis Villeneuve",
    "categoria": "BEST PICTURE",
    "vencedor": 0,
    "comentarios": "Filme não ganhou Oscar de Melhor Filme, apesar de ser visualmente deslumbrante e tecnicamente impressionante. A cinematografia de Roger Deakins e a direção de Villeneuve mereciam mais reconhecimento."
  },
  {
    "_id": { "$oid": "5fcae4a2e9b6b2e8c7dba1c2" },
    "id_registro": 2,
    "nome_do_filme": "Her",
    "ano_filmagem": 2013,
    "direcao": "Spike Jonze",
    "categoria": "BEST PICTURE",
    "vencedor": 0,
    "comentarios": "Apesar de sua abordagem única sobre o amor e solidão na era digital, o filme foi subestimado no Oscar. A atuação de Joaquin Phoenix e o roteiro criativo de Jonze mereciam mais reconhecimento."
  },
  {
    "_id": { "$oid": "5fcae4a2e9b6b2e8c7dba1c3" },
    "id_registro": 3,
    "nome_do_filme": "Fight Club",
    "ano_filmagem": 1999,
    "direcao": "David Fincher",
    "categoria": "BEST PICTURE",
    "vencedor": 0,
    "comentarios": "Embora tenha se tornado um clássico cult, Fight Club não foi reconhecido pelo Oscar nas principais categorias. Sua abordagem única e a direção impecável de Fincher mereciam mais visibilidade."
  }
])

```

---

- Denzel Washington já ganhou algum Oscar?

  R: Sim, duas vezes.

```
db.oscar.find({nome_do_indicado: "Denzel Washington", vencedor: 1})

{
  "_id": {
    "$oid": "6818b0f66a44c95738f4ce9b"
  },
  "id_registro": 6794,
  "ano_filmagem": 1989,
  "ano_cerimonia": 1990,
  "cerimonia": 62,
  "categoria": "ACTOR IN A SUPPORTING ROLE",
  "nome_do_indicado": "Denzel Washington",
  "nome_do_filme": "Glory",
  "vencedor": 1
}

{
  "_id": {
    "$oid": "6818b0f76a44c95738f4d3c9"
  },
  "id_registro": 8120,
  "ano_filmagem": 2001,
  "ano_cerimonia": 2002,
  "cerimonia": 74,
  "categoria": "ACTOR IN A LEADING ROLE",
  "nome_do_indicado": "Denzel Washington",
  "nome_do_filme": "Training Day",
  "vencedor": 1
}
```

---

- Quais os filmes que ganharam o Oscar de Melhor Filme? 

 R: 🏆 **Filmes Vencedores do Oscar de Melhor Filme (1929–2025)**
- **1929**: *Asas* (*Wings*)
- **1930**: *Melodia na Broadway* (*The Broadway Melody*)
- **1931**: *Nada de Novo no Front* (*All Quiet on the Western Front*)
- **1932**: *Cimarron*
- **1933**: *Grande Hotel* (*Grand Hotel*)
- **1934**: *Cavalgada* (*Cavalcade*)
- **1935**: *Aconteceu Naquela Noite* (*It Happened One Night*)
- **1936**: *O Grande Motim* (*Mutiny on the Bounty*)
- **1937**: *Ziegfeld – O Criador de Estrelas* (*The Great Ziegfeld*)
- **1938**: *A Vida de Emile Zola* (*The Life of Emile Zola*)
- **1939**: *Do Mundo Nada Se Leva* (*You Can't Take It with You*)
- **1940**: *...E o Vento Levou* (*Gone with the Wind*)
- **1941**: *Rebecca – A Mulher Inesquecível* (*Rebecca*)
- **1942**: *Como Era Verde o Meu Vale* (*How Green Was My Valley*)
- **1943**: *Rosa de Esperança* (*Mrs. Miniver*)
- **1944**: *Casablanca*
- **1945**: *O Bom Pastor* (*Going My Way*)
- **1946**: *Farrapo Humano* (*The Lost Weekend*)
- **1947**: *Os Melhores Anos de Nossas Vidas* (*The Best Years of Our Lives*)
- **1948**: *A Luz É Para Todos* (*Gentleman's Agreement*)
- **1949**: *Hamlet*
- **1950**: *A Grande Ilusão* (*All the King's Men*)
- **1951**: *A Malvada* (*All About Eve*)
- **1952**: *Sinfonia de Paris* (*An American in Paris*)
- **1953**: *O Maior Espetáculo da Terra* (*The Greatest Show on Earth*)
- **1954**: *A Um Passo da Eternidade* (*From Here to Eternity*)
- **1955**: *Sindicato de Ladrões* (*On the Waterfront*)
- **1956**: *Marty*
- **1957**: *A Volta ao Mundo em 80 Dias* (*Around the World in 80 Days*)
- **1958**: *A Ponte do Rio Kwai* (*The Bridge on the River Kwai*)
- **1959**: *Gigi*
- **1960**: *Ben-Hur*
- **1961**: *Se Meu Apartamento Falasse* (*The Apartment*)
- **1962**: *Amor, Sublime Amor* (*West Side Story*)
- **1963**: *Lawrence da Arábia* (*Lawrence of Arabia*)
- **1964**: *As Aventuras de Tom Jones* (*Tom Jones*)
- **1965**: *Minha Bela Dama* (*My Fair Lady*)
- **1966**: *A Noviça Rebelde* (*The Sound of Music*)
- **1967**: *O Homem Que Não Vendeu Sua Alma* (*A Man for All Seasons*)
- **1968**: *No Calor da Noite* (*In the Heat of the Night*)
- **1969**: *Oliver!*
- **1970**: *Perdidos na Noite* (*Midnight Cowboy*)
- **1971**: *Patton, Rebelde ou Herói?* (*Patton*)
- **1972**: *Operação França* (*The French Connection*)
- **1973**: *O Poderoso Chefão* (*The Godfather*)
- **1974**: *Golpe de Mestre* (*The Sting*)
- **1975**: *O Poderoso Chefão: Parte II* (*The Godfather Part II*)
- **1976**: *Um Estranho no Ninho* (*One Flew Over the Cuckoo's Nest*)
- **1977**: *Rocky, um Lutador* (*Rocky*)
- **1978**: *Noivo Neurótico, Noiva Nervosa* (*Annie Hall*)
- **1979**: *O Franco-Atirador* (*The Deer Hunter*)
- **1980**: *Kramer Versus Kramer* (*Kramer vs. Kramer*)
- **1981**: *Gente Como a Gente* (*Ordinary People*)
- **1982**: *Carruagens de Fogo* (*Chariots of Fire*)
- **1983**: *Gandhi*
- **1984**: *Laços de Ternura* (*Terms of Endearment*)
- **1985**: *Amadeus*
- **1986**: *Entre Dois Amores* (*Out of Africa*)
- **1987**: *Platoon*
- **1988**: *O Último Imperador* (*The Last Emperor*)
- **1989**: *Rain Man*
- **1990**: *Conduzindo Miss Daisy* (*Driving Miss Daisy*)
- **1991**: *Dança com Lobos* (*Dances with Wolves*)
- **1992**: *O Silêncio dos Inocentes* (*The Silence of the Lambs*)
- **1993**: *Os Imperdoáveis* (*Unforgiven*)
- **1994**: *A Lista de Schindler* (*Schindler's List*)
- **1995**: *Forrest Gump – O Contador de Histórias* (*Forrest Gump*)
- **1996**: *Coração Valente* (*Braveheart*)
- **1997**: *O Paciente Inglês* (*The English Patient*)
- **1998**: *Titanic*
- **1999**: *Shakespeare Apaixonado* (*Shakespeare in Love*)
- **2000**: *Beleza Americana* (*American Beauty*)
- **2001**: *Gladiador* (*Gladiator*)
- **2002**: *Uma Mente Brilhante* (*A Beautiful Mind*)
- **2003**: *Chicago*
- **2004**: *O Senhor dos Anéis: O Retorno do Rei* (*The Lord of the Rings: The Return of the King*)
- **2005**: *Menina de Ouro* (*Million Dollar Baby*)
- **2006**: *Crash – No Limite* (*Crash*)
- **2007**: *Os Infiltrados* (*The Departed*)
- **2008**: *Onde os Fracos Não Têm Vez* (*No Country for Old Men*)
- **2009**: *Quem Quer Ser um Milionário?* (*Slumdog Millionaire*)
- **2010**: *Guerra ao Terror* (*The Hurt Locker*)
- **2011**: *O Discurso do Rei* (*The King's Speech*)
- **2012**: *O Artista* (*The Artist*)
- **2013**: *Argo*
- **2014**: *12 Anos de Escravidão* (*12 Years a Slave*)
- **2015**: *Birdman ou (A Inesperada Virtude da Ignorância)* (*Birdman or (The Unexpected Virtue of Ignorance)*)
- **2016**: *Spotlight: Segredos Revelados* (*Spotlight*)
- **2017**: *Moonlight: Sob a Luz do Luar* (*Moonlight*)
- **2018**: *A Forma da Água* (*The Shape of Water*)
- **2019**: *Green Book: O Guia* (*Green Book*)
- **2020**: *Parasita* (*Parasite*)
- **2021**: *Nomadland*
- **2022**: *No Ritmo do Coração* (*CODA*)
- **2023**: *Tudo em Todo o Lugar ao Mesmo Tempo* (*Everything Everywhere All at Once*)
- **2024**: *Oppenheimer*
- **2025**: *Anora*

Q:

```
db.oscar.find({categoria: "BEST PICTURE", vencedor: 1})
```

---

- Sidney Poitier foi o primeiro ator negro a ser indicado ao Oscar. Em que ano ele foi indicado? Por qual filme?

 R: Sindey Poitier foi indicado duas vezes, nos anos 1959 e 1964. Pelos filmes: "The Defiant Ones” e "Lilies of the Field”. Ganhando na cerimonia de 1964.

```
db.oscar.find ({nome_do_indicado: "Sidney Poitier"})

{
  "_id": {
    "$oid": "6818b0f66a44c95738f4c137"
  },
  "id_registro": 3366,
  "ano_filmagem": 1958,
  "ano_cerimonia": 1959,
  "cerimonia": 31,
  "categoria": "ACTOR",
  "nome_do_indicado": "Sidney Poitier",
  "nome_do_filme": "The Defiant Ones",
  "vencedor": 0
}

{
  "_id": {
    "$oid": "6818b0f66a44c95738f4c38a"
  },
  "id_registro": 3961,
  "ano_filmagem": 1963,
  "ano_cerimonia": 1964,
  "cerimonia": 36,
  "categoria": "ACTOR",
  "nome_do_indicado": "Sidney Poitier",
  "nome_do_filme": "Lilies of the Field",
  "vencedor": 1
}
```

---

- Quais os filmes que ganharam o Oscar de Melhor Filme e Melhor Diretor na mesma cerimonia?

R: 🏆 **Filmes que venceram Melhor Filme e Melhor Diretor no mesmo ano:**

- **2024**: *Oppenheimer* – Direção: Christopher Nolan
- **1988**: *O Último Imperador* (*The Last Emperor*) – Direção: Bernardo Bertolucci
- **1987**: *Platoon* – Direção: Oliver Stone
- **1985**: *Amadeus* – Direção: Miloš Forman
- **1976**: *Um Estranho no Ninho* (*One Flew Over the Cuckoo’s Nest*) – Direção: Miloš Forman

Q: (Esse foi um pouco mais dificil)

```
// Perto de documents,clica na aba "Aggregations" 
//Abaixo disso, clica no botão “+ ADD STAGE”.
//Etapa 1: $match
Clica em + ADD STAGE

Escreve:

{
  categoria: { $in: ["BEST PICTURE", "DIRECTOR"] },
  vencedor: 1
}

// Etapa 2: $group
Clica em + ADD STAGE

No campo de código, coloca:
{
  _id: {
    filme: "$nome_do_filme",
    ano: "$ano_cerimonia"
  },
  categorias: { $addToSet: "$categoria" }
}

//Etapa 3: $match (de novo!)
+ ADD STAGE

Escreve:{
  categorias: { $all: ["BEST PICTURE", "DIRECTOR"] }
}

//Agora é só clicar no botão "Run" (lá em cima à direita).
```

---
