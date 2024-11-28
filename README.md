# SSC0108 - Prática Sistemas Digitais

[Aula 6: Timers and real clock.](./lab5.pdf)

### ALUNOS

|        Nome                         |    NUSP   |       
|:-----------------------------------:|:---------:|  
|   Artur Domitti Camargo             |  15441661 |   
|   Lucas Mello Ciosaki       	      |  14591305 |   
|   Lucas Alves da Silva		         |  11819553  | 

### PART IV

VHDL CODE

```
LIBRARY IEEE;
USE ieee.std_logic_1164.ALL;
USE ieee.numeric_std.ALL;

ENTITY morse_code IS
    PORT(
        Clock, Start, Reset: IN std_logic;
        Letter: IN std_logic_vector(2 downto 0);
        Led: OUT std_logic
    );
END morse_code;

ARCHITECTURE Behavioral OF morse_code IS
    SIGNAL morse: std_logic_vector(10 downto 0);
    SIGNAL morse_current: std_logic := '0';
    SIGNAL counter: INTEGER := 0;
    SIGNAL enabled: std_logic := '0';
	 SIGNAL last: std_logic:= '0';

BEGIN
    PROCESS(Start, Letter, Clock, Reset)
    BEGIN
        IF rising_edge(Clock) THEN
            IF Reset = '1' THEN
                counter <= 0;
                enabled <= '0';
                Led <= '0'; -- Desligar o LED no reset
				END IF;

            IF enabled = '0' then
                CASE Letter IS
                    WHEN "000" => morse <= "10111000000"; -- A
                    WHEN "001" => morse <= "11101010100"; -- B
                    WHEN "010" => morse <= "11101011101"; -- C
                    WHEN "011" => morse <= "11101010000"; -- D
                    WHEN "100" => morse <= "10000000000"; -- E
                    WHEN "101" => morse <= "10101110100"; -- F
                    WHEN "110" => morse <= "11101110100"; -- G
                    WHEN "111" => morse <= "10101010000"; -- H
                    WHEN OTHERS => morse <= (OTHERS => '0'); -- Default value
                END CASE;
				END IF;
				
            IF Start = '1' AND Reset = '0' THEN
                enabled <= '1';
            END IF;

            IF enabled = '1' THEN
               counter <= counter + 1;
					IF counter = 25000000 THEN
							IF(last = '0') THEN
							  morse_current <= morse(10);
							  morse <= morse(9 downto 0) & '0';
							  Led <= morse_current;
							  counter <= 0;

									-- Verificar se a transmissão terminou
							  IF morse = "00000000000" THEN
									last <= '1'; -- Desativar quando a transmissão terminar
							  END IF;
							ELSE
								counter <= 0;
								Led <= '0';
								enabled <= '0';
								last <= '0';
							END IF;	
                END IF;
             END IF;
          END IF;

    END PROCESS;

END Behavioral;
```
