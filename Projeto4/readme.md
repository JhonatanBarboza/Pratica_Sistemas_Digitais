# Praticas em Sistemas Digitais - SSC0108

Projeto 4: 

# Introdução

Versão do Quartus: Quartus Prime 21.1 <br>
Versão ModelSim: ModelSim - Intel FPGA Starter Edition 10.5b <br>

# Resultados

A seguir estão os resultados obtidos e os codigos VHDL utilizados em cada uma das partes do Projeto.

## Part 1

VHDL:
```VHDL
library ieee;
use ieee.std_logic_1164.all;

entity part1 is
	port(address : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
		clock : IN STD_LOGIC := '1';
		data : IN STD_LOGIC_VECTOR (3 DOWNTO 0);
		wren : IN STD_LOGIC ;
		q : OUT STD_LOGIC_VECTOR (3 DOWNTO 0) );
end part1;

architecture be of part1 is
	
	component ram32x4
		PORT ( address : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
				clock : IN STD_LOGIC := '1';
				data : IN STD_LOGIC_VECTOR (3 DOWNTO 0);
				wren : IN STD_LOGIC ;
				q : OUT STD_LOGIC_VECTOR (3 DOWNTO 0) );
	end component;
	
begin

	inst1: ram32x4
	port map(address => address,
				clock => clock,
				data => data,
				wren => wren,
				q => q);

end be;
```

Quantidade de blocos de memória:

![Captura de tela 2024-10-02 150334](https://github.com/user-attachments/assets/9241f033-fe86-43d8-9b12-19a32682b78d)
![Capturar](https://github.com/user-attachments/assets/aa27b310-7eb5-43c3-9234-c00b2f925c54)

Simulação:

## Part 2

VHDL:
```VHDL
library ieee;
use ieee.std_logic_1164.all;

entity part2 is
	port(address : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
		clock : IN STD_LOGIC := '1';
		data : IN STD_LOGIC_VECTOR (3 DOWNTO 0);
		wren : IN STD_LOGIC ;
		out0 : out std_logic_vector(6 downto 0);
		out2 : out std_logic_vector(6 downto 0);
		out4 : out std_logic_vector(6 downto 0);
		out5 : out std_logic_vector(6 downto 0));
end part2;

architecture be of part2 is
	
	component ram32x4
		PORT ( address : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
				clock : IN STD_LOGIC := '1';
				data : IN STD_LOGIC_VECTOR (3 DOWNTO 0);
				wren : IN STD_LOGIC ;
				q : OUT STD_LOGIC_VECTOR (3 DOWNTO 0) );
	end component;
	
	component display
		port(
			S0 :  IN  STD_LOGIC;
			S1 :  IN  STD_LOGIC;
			S2 :  IN  STD_LOGIC;
			S3 :  IN  STD_LOGIC;
			p0 :  OUT  STD_LOGIC;
			p1 :  OUT  STD_LOGIC;
			p2 :  OUT  STD_LOGIC;
			p3 :  OUT  STD_LOGIC;
			p4 :  OUT  STD_LOGIC;
			p5 :  OUT  STD_LOGIC;
			p6 :  OUT  STD_LOGIC
		);
	end component;
		
	signal q: std_logic_vector(3 downto 0);
	
	
begin

	inst1: ram32x4
	port map(address => address,
				clock => clock,
				data => data,
				wren => wren,
				q => q);
				
	display_ad1: display
	port map(S0 => address(0),
				S1 => address(1),
				S2 => address(2),
				S3 => address(3),
				p0 => out4(0),
				p1 => out4(1),
				p2 => out4(2),
				p3 => out4(3),
				p4 => out4(4),
				p5 => out4(5),
				p6 => out4(6));
				
	display_ad2: display
	port map(S0 => address(4),
				S1 => '0',
				S2 => '0',
				S3 => '0',
				p0 => out5(0),
				p1 => out5(1),
				p2 => out5(2),
				p3 => out5(3),
				p4 => out5(4),
				p5 => out5(5),
				p6 => out5(6));
				
	display_data: display
	port map(S0 => data(0),
				S1 => data(1),
				S2 => data(2),
				S3 => data(3),
				p0 => out2(0),
				p1 => out2(1),
				p2 => out2(2),
				p3 => out2(3),
				p4 => out2(4),
				p5 => out2(5),
				p6 => out2(6));
				
	display_read: display
	port map(S0 => q(0),
				S1 => q(1),
				S2 => q(2),
				S3 => q(3),
				p0 => out0(0),
				p1 => out0(1),
				p2 => out0(2),
				p3 => out0(3),
				p4 => out0(4),
				p5 => out0(5),
				p6 => out0(6));
	

end be;
```

## Part 3

VHDL:
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_unsigned.all;

entity part3_verdadeiro is
    port (
        clk : in std_logic; -- KEY0
        write_enable : in std_logic; -- SW9
        address : in std_logic_vector(4 downto 0);  -- SW8-4
        data_in : in std_logic_vector(3 downto 0);  -- SW3-0
        out0 : out std_logic_vector(6 downto 0); -- data out
        out2 : out std_logic_vector(6 downto 0); -- data in
        out4 : out std_logic_vector(6 downto 0); -- address hex 4
        out5 : out std_logic_vector(6 downto 0) -- address hex 5
    );
end part3_verdadeiro;

architecture behavior of part3_verdadeiro is
    type mem is array (0 to 31) of std_logic_vector(3 downto 0);
    signal mem_array : mem;
    signal temp_out  : std_logic_vector(3 downto 0);
	 
	 component display
		PORT
		(
			S0 :  IN  STD_LOGIC;
			S1 :  IN  STD_LOGIC;
			S2 :  IN  STD_LOGIC;
			S3 :  IN  STD_LOGIC;
			p0 :  OUT  STD_LOGIC;
			p1 :  OUT  STD_LOGIC;
			p2 :  OUT  STD_LOGIC;
			p3 :  OUT  STD_LOGIC;
			p4 :  OUT  STD_LOGIC;
			p5 :  OUT  STD_LOGIC;
			p6 :  OUT  STD_LOGIC
		);
	end component;
	 
begin
    process (clk)
    begin
        if rising_edge(clk) then
            if write_enable = '1' then
                mem_array(conv_integer(address)) <= data_in;
					 temp_out <= data_in;
            else
					temp_out <= mem_array(conv_integer(address));
				end if;
        end if;
    end process;

    display_data: display
	 port map(S0 => data_in(0),
				S1 => data_in(1),
				S2 => data_in(2),
				S3 => data_in(3),
				p0 => out2(0),
				p1 => out2(1),
				p2 => out2(2),
				p3 => out2(3),
				p4 => out2(4),
				p5 => out2(5),
				p6 => out2(6));
				
	 display_read: display
	 port map(S0 => temp_out(0),
				S1 => temp_out(1),
				S2 => temp_out(2),
				S3 => temp_out(3),
				p0 => out0(0),
				p1 => out0(1),
				p2 => out0(2),
				p3 => out0(3),
				p4 => out0(4),
				p5 => out0(5),
				p6 => out0(6));
				
	 display_ad1: display
	 port map(S0 => address(0),
				S1 => address(1),
				S2 => address(2),
				S3 => address(3),
				p0 => out4(0),
				p1 => out4(1),
				p2 => out4(2),
				p3 => out4(3),
				p4 => out4(4),
				p5 => out4(5),
				p6 => out4(6));
				
	 display_ad2: display
	 port map(S0 => address(4),
				S1 => '0',
				S2 => '0',
				S3 => '0',
				p0 => out5(0),
				p1 => out5(1),
				p2 => out5(2),
				p3 => out5(3),
				p4 => out5(4),
				p5 => out5(5),
				p6 => out5(6));

end behavior;
```

## Part 4

VHDL:
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity part4 is
	port(wraddress : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
		clock : IN STD_LOGIC;
		data : IN STD_LOGIC_VECTOR (3 DOWNTO 0);
		wren : IN STD_LOGIC ;
		rst  : in std_logic;
		out0 : out std_logic_vector(6 downto 0);
		out1 : out std_logic_vector(6 downto 0);
		out2 : out std_logic_vector(6 downto 0);
		out3 : out std_logic_vector(6 downto 0);
		out4 : out std_logic_vector(6 downto 0);
		out5 : out std_logic_vector(6 downto 0));
end part4;

architecture be of part4 is
	
	component ram32x4
		PORT ( wraddress : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
				 rdaddress : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
				clock : IN STD_LOGIC := '1';
				data : IN STD_LOGIC_VECTOR (3 DOWNTO 0);
				wren : IN STD_LOGIC ;
				q : OUT STD_LOGIC_VECTOR (3 DOWNTO 0) );
	end component;
	
	component display
		port(
			S0 :  IN  STD_LOGIC;
			S1 :  IN  STD_LOGIC;
			S2 :  IN  STD_LOGIC;
			S3 :  IN  STD_LOGIC;
			p0 :  OUT  STD_LOGIC;
			p1 :  OUT  STD_LOGIC;
			p2 :  OUT  STD_LOGIC;
			p3 :  OUT  STD_LOGIC;
			p4 :  OUT  STD_LOGIC;
			p5 :  OUT  STD_LOGIC;
			p6 :  OUT  STD_LOGIC
		);
	end component;
		
	signal q: std_logic_vector(3 downto 0);
	signal counter: std_logic_vector(27 downto 0) := (others => '0');
	signal counter1: std_logic_vector(4 downto 0) := (others => '0');
	signal clk_1sec: std_logic := '0';
	
	
begin

	process(clock)
	begin
		if(rising_edge(clock)) then
			if(unsigned(counter) = 25000000) then
				counter <= (others => '0');
				clk_1sec <= not clk_1sec;
			else
				counter <= std_logic_vector(unsigned(counter)+1);
			end if;
		end if;
	end process;
	
	process(clk_1sec, rst)
	begin
		if(rst = '0') then
			counter1 <= (others => '0');
		elsif(rising_edge(clk_1sec)) then
			if(unsigned(counter1) = 31) then
				counter1 <= (others =>'0');
			else
				counter1 <= std_logic_vector(unsigned(counter1)+1);
			end if;
		end if;
	end process;
			

	inst1: ram32x4
	port map(wraddress => wraddress,
				rdaddress => counter1,
				clock => clock,
				data => data,
				wren => wren,
				q => q);
				
	display_ad1: display
	port map(S0 => wraddress(0),
				S1 => wraddress(1),
				S2 => wraddress(2),
				S3 => wraddress(3),
				p0 => out4(0),
				p1 => out4(1),
				p2 => out4(2),
				p3 => out4(3),
				p4 => out4(4),
				p5 => out4(5),
				p6 => out4(6));
				
	display_ad2: display
	port map(S0 => wraddress(4),
				S1 => '0',
				S2 => '0',
				S3 => '0',
				p0 => out5(0),
				p1 => out5(1),
				p2 => out5(2),
				p3 => out5(3),
				p4 => out5(4),
				p5 => out5(5),
				p6 => out5(6));
				
	display_data: display
	port map(S0 => data(0),
				S1 => data(1),
				S2 => data(2),
				S3 => data(3),
				p0 => out1(0),
				p1 => out1(1),
				p2 => out1(2),
				p3 => out1(3),
				p4 => out1(4),
				p5 => out1(5),
				p6 => out1(6));
				
	display_read: display
	port map(S0 => q(0),
				S1 => q(1),
				S2 => q(2),
				S3 => q(3),
				p0 => out0(0),
				p1 => out0(1),
				p2 => out0(2),
				p3 => out0(3),
				p4 => out0(4),
				p5 => out0(5),
				p6 => out0(6));
				
	display_read_ad1: display
	port map(S0 => counter1(0),
				S1 => counter1(1),
				S2 => counter1(2),
				S3 => counter1(3),
				p0 => out2(0),
				p1 => out2(1),
				p2 => out2(2),
				p3 => out2(3),
				p4 => out2(4),
				p5 => out2(5),
				p6 => out2(6));
	
	display_read_ad2: display
	port map(S0 => counter1(4),
				S1 => '0',
				S2 => '0',
				S3 => '0',
				p0 => out3(0),
				p1 => out3(1),
				p2 => out3(2),
				p3 => out3(3),
				p4 => out3(4),
				p5 => out3(5),
				p6 => out3(6));
	

end be;
```




