#Redisgo - Atividade Prática – Redis

- Abaixo segue o código fonte pra ficar fácil de passar o olho.
- Abaixo segue também a saída do programa para ter uma ideia de como ficou o resultado.

- O jogo foi desenvolvido em Java com maven. Usei a dependência maven do [redisson](https://redisson.org/) como client para acessar o Redis que rodou em uma imagem Docker em uma VM Ubuntu.
- O Jar do programa `redinsgo.jar`, bem como todo o projeto seguem juntos, caso queira executar.
- O servidor e a porta do servidor Redis podem ser passados por parametro, exemplo: `java -jar -Dserver=192.168.2.4 -Dport=6379 redinsgo.jar`. 
Por padrão, se não passar nada, vai conectar no `localhost` e na porta `6379`.

# O que foi usado do Redis via redisson
Hashes, executa o `HSET` e `HGET`:
```java
RMap<String, String> user = redisson.getMap(key);	
```

Set `SADD` e `SMEMBERS`:
```java
RSet<Integer> cartela = redisson.getSet(key);
...
cartela.addAll(cartelaNumetos);
```

SRANDMEMBER:
```
int n = PEDRAS_POR_CARTELA;
Set<Integer> cartelaNumetos = bingoNumbers.random(n);
```

INCR:
```java
long score = redisson.getAtomicLong(key).incrementAndGet();
if (score >= SCORE_VENCEDOR) {
    vencedor = i;
    break;
}
```

#Saída do programa

```
Jogaremos 15 vezes para ver os resultados.
-----------------------------------------------------------
Jogada 1
O vendedor do BINGO com 15 acertos foi: user:39
-----------------------------------------------------------
Jogada 2
O vendedor do BINGO com 15 acertos foi: user:07
-----------------------------------------------------------
Jogada 3
O vendedor do BINGO com 15 acertos foi: user:07
-----------------------------------------------------------
Jogada 4
O vendedor do BINGO com 15 acertos foi: user:06
-----------------------------------------------------------
Jogada 5
O vendedor do BINGO com 15 acertos foi: user:33
-----------------------------------------------------------
Jogada 6
O vendedor do BINGO com 15 acertos foi: user:06
-----------------------------------------------------------
Jogada 7
O vendedor do BINGO com 15 acertos foi: user:49
-----------------------------------------------------------
Jogada 8
O vendedor do BINGO com 15 acertos foi: user:50
-----------------------------------------------------------
Jogada 9
O vendedor do BINGO com 15 acertos foi: user:37
-----------------------------------------------------------
Jogada 10
O vendedor do BINGO com 15 acertos foi: user:22
-----------------------------------------------------------
Jogada 11
O vendedor do BINGO com 15 acertos foi: user:12
-----------------------------------------------------------
Jogada 12
O vendedor do BINGO com 15 acertos foi: user:09
-----------------------------------------------------------
Jogada 13
O vendedor do BINGO com 15 acertos foi: user:22
-----------------------------------------------------------
Jogada 14
O vendedor do BINGO com 15 acertos foi: user:30
-----------------------------------------------------------
Jogada 15
O vendedor do BINGO com 15 acertos foi: user:29
-----------------------------------------------------------
FIM!

```

#Código fonte:
```java
package br.com.koch.marcio;

import java.text.MessageFormat;
import java.util.Iterator;
import java.util.Set;

import org.redisson.Redisson;
import org.redisson.api.RMap;
import org.redisson.api.RSet;
import org.redisson.api.RedissonClient;
import org.redisson.config.Config;

public class Redinsgo {
	
	private RedissonClient redisson;
	
	private static final int NUMERO_PEDRAS = 99;
	private static final int PEDRAS_POR_CARTELA = 15;
	private static final int SCORE_VENCEDOR = 15;
	private static final int QUANTIDADE_JOGADORES = 50;
	
	public Redinsgo(RedissonClient redisson) {
		this.redisson = redisson;
	}
	
	private void createData(int quantity) {
		createUsers(quantity);
		createCartelas(quantity);
		createScores(quantity);
	}
	
	public void createUsers(int quantity) {
		for (int i = 1; i <= quantity; i++) {
			String key = formatKey("user", i);
			
			RMap<String, String> user = redisson.getMap(key);	
	        user.put("name", key);
	        
	        key = formatKey("cartela", i);
	        user.put("bcartela", key);
	        
	        key = formatKey("score", i);
	        user.put("bscore", key);
		}
	}
	
	public void createCartelas(int quantity) {
		RSet<Integer> bingoNumbers = redisson.getSet("bingoNumbers");
		bingoNumbers.clear();
		for (int i = 1; i <= NUMERO_PEDRAS; i++) {
			bingoNumbers.add(i);
		}
		
		for (int i = 1; i <= quantity; i++) {
			String key = formatKey("cartela", i);
			
			RSet<Integer> cartela = redisson.getSet(key);
			cartela.clear();
			
			int n = PEDRAS_POR_CARTELA;
			Set<Integer> cartelaNumetos = bingoNumbers.random(n);
			
			if (cartelaNumetos.size() != n) {
				throw new IllegalStateException(MessageFormat.format("Deveria ter retornado '{0}' numeros para a cartela, mas retornou '{1}'.", n, cartelaNumetos.size()));
			}
			
			cartela.addAll(cartelaNumetos);
		}
	}
	
	public void createScores(int quantity) {
		for (int i = 1; i <= quantity; i++) {
			String key = formatKey("score", i);
			redisson.getAtomicLong(key).set(0);			
		}
	}
	
	private String formatKey(String str, int value) {
		String result = String.format("%s:%02d", str, value);
		return result;
	}
	
	public void play() {
		int quantity = QUANTIDADE_JOGADORES;
		createData(quantity);
		
		runPlay(quantity);
	}

	private void runPlay(int quantity) {
		RSet<Integer> bingoNumbers = redisson.getSet("bingoNumbers");
		
		Integer pedra = bingoNumbers.removeRandom();
		int vencedor = -1;
		while(pedra != null && vencedor == -1) {
			
			int i = 1;
			while (i <= quantity && vencedor == -1) {
				String key = formatKey("cartela", i);
				RSet<Integer> cartela = redisson.getSet(key);
				
				Iterator<Integer> cartelaIterator = cartela.iterator();
				while (cartelaIterator.hasNext()) {
					
					Integer value = cartelaIterator.next();
					if (pedra.equals(value)) {
						key = formatKey("score", i);
						long score = redisson.getAtomicLong(key).incrementAndGet();
						if (score >= SCORE_VENCEDOR) {
							vencedor = i;
							break;
						}
					}
					
				} // while
				
				i++;
			} // while
			
			pedra = bingoNumbers.removeRandom();
			
		}
		
		
		if (vencedor == -1) {
			System.out.println("Nao houve vencedor :(");
			return;
		}
		
		String key = formatKey("user", vencedor);
		RMap<String, String> user = redisson.getMap(key);	

		System.out.println("O vendedor do BINGO com 15 acertos foi: " + user.get("name"));		
		
	}

	public static void main(String[] args) {
		System.out.println("Comecando o programa Redinsgo. " + 
				"O servidor e a porta do servidor Redis podem ser passados por parametro, " + 
				"exemplo: java -jar -Dserver=192.168.2.4 -Dport=6379 redinsgo.jar");
		
		Config config = new Config();
		
		String server = System.getProperty("server", "localhost");
		String port = System.getProperty("port", "6379");
		
		// 192.168.2.4
		String connUrl = MessageFormat.format("redis://{0}:{1}", server, port);
		System.out.println("Conectando-se ao servidor:" + connUrl);
		
        config.useSingleServer().setAddress(connUrl);
        RedissonClient redisson = Redisson.create(config);
		
        System.out.println(" ");
		System.out.println("Jogaremos 15 vezes para ver os resultados.");
		
		for (int i = 1; i <= 15; i++) {
			System.out.println("-----------------------------------------------------------");
			System.out.println("Jogada " + i);
			
			Redinsgo jogo = new Redinsgo(redisson);
			jogo.play();
			
		}
		
		redisson.shutdown();
		
		System.out.println("-----------------------------------------------------------");
	    System.out.println("FIM!");

	}

}

```