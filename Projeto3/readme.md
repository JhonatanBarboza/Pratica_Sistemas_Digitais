```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity morse is
	port(sw: in std_logic_vector(2 downto 0);
		  clk, b0, b1: in std_logic;
		  led: out std_logic);
end entity;

architecture be of morse is

signal clk_05: std_logic := '0';
signal counter: std_logic_vector(26 downto 0) := (others => '0'); 
signal counter1: std_logic_vector(3 downto 0) := (others => '0'); 
constant max: integer range 0 to 12500000-1 := 12500000-1;
signal a: std_logic_vector(11 downto 0) := "010111000000";
signal b: std_logic_vector(11 downto 0) := "011101010100";
signal c: std_logic_vector(11 downto 0) := "011101011101";
signal d: std_logic_vector(11 downto 0) := "011101010000";
signal e: std_logic_vector(11 downto 0) := "010000000000";
signal f: std_logic_vector(11 downto 0) := "010101110100";
signal g: std_logic_vector(11 downto 0) := "011101110100";
signal h: std_logic_vector(11 downto 0) := "010101010000";
signal a_b: integer range 0 to 5 := 5;
signal b_b: integer range 0 to 9 := 9;
signal c_b: integer range 0 to 12 := 12;
signal d_b: integer range 0 to 8 := 8;
signal e_b: integer range 0 to 2 := 2;
signal f_b: integer range 0 to 10 := 10;
signal g_b: integer range 0 to 10 := 10;
signal h_b: integer range 0 to 8 := 8;
signal res_b: integer range 0 to 12 := 0;

signal res: std_logic_vector(11 downto 0) := "000000000000";


begin

	process(clk)
	begin
		if(rising_edge(clk)) then
			if(unsigned(counter) = max) then
				counter <= (others => '0');
				clk_05 <= not clk_05;
			else
				counter <= std_logic_vector(unsigned(counter)+1);
			end if;
		end if;
	end process;
					
	
	process(clk_05, sw, b0, b1)
	begin
		if(b0 = '0') then
			res <= (others => '0');
		elsif(b1 = '1' and rising_edge(clk_05)) then
			case (sw) is
				when "000" =>
					res <= a;
					res_b <= a_b;
				when "001" =>
					res <= b;
					res_b <= b_b;
				when "010" =>
					res <= c;
					res_b <= c_b;
				when "011" =>
					res <= d;
					res_b <= d_b;
				when "100" =>
					res <= e;
					res_b <= e_b;
				when "101" =>
					res <= f;
					res_b <= f_b;
				when "110" =>
					res <= g;
					res_b <= g_b;
				when "111" =>
					res <= h;
					res_b <= h_b;
			end case;
			
			
		elsif(rising_edge(clk_05)) then
			if(unsigned(counter1) = res_b) then
				counter1 <= (others => '0');
			else
				counter1 <= std_logic_vector(unsigned(counter1)+1);
				res <= std_logic_vector(shift_left(unsigned(res), 1));
			end if;
		end if;
	end process;
			
	
	led <= res(11);
	
end be;
```

```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity morse is
	port(sw: in std_logic_vector(2 downto 0);
		  clk, b0, b1: in std_logic;
		  led: out std_logic);
end entity;

architecture be of morse is

signal clk_05: std_logic := '0';
signal counter: std_logic_vector(26 downto 0) := (others => '0'); 
signal counter1: std_logic_vector(3 downto 0) := (others => '0'); 
constant max: integer range 0 to 12500000-1 := 12500000-1;
signal a: std_logic_vector(11 downto 0) := "010111000000";
signal b: std_logic_vector(11 downto 0) := "011101010100";
signal c: std_logic_vector(11 downto 0) := "011101011101";
signal d: std_logic_vector(11 downto 0) := "011101010000";
signal e: std_logic_vector(11 downto 0) := "010000000000";
signal f: std_logic_vector(11 downto 0) := "010101110100";
signal g: std_logic_vector(11 downto 0) := "011101110100";
signal h: std_logic_vector(11 downto 0) := "010101010000";
signal flag: std_logic;
signal a_b: integer range 0 to 5 := 5;
signal b_b: integer range 0 to 9 := 9;
signal c_b: integer range 0 to 12 := 12;
signal d_b: integer range 0 to 8 := 8;
signal e_b: integer range 0 to 2 := 2;
signal f_b: integer range 0 to 10 := 10;
signal g_b: integer range 0 to 10 := 10;
signal h_b: integer range 0 to 8 := 8;
signal res_b: integer range 0 to 12 := 0;

signal res: std_logic_vector(11 downto 0) := "000000000000";


begin

	process(clk)
	begin
		if(rising_edge(clk)) then
			if(unsigned(counter) = max) then
				counter <= (others => '0');
				clk_05 <= not clk_05;
			else
				counter <= std_logic_vector(unsigned(counter)+1);
			end if;
		end if;
	end process;
					
	
	process(clk_05, sw, b0, b1)
	begin
		if(b0 = '0') then
			res <= (others => '0');
		elsif(b1 = '1' and rising_edge(clk_05)) then
			case (sw) is
				when "000" =>
					res <= a;
					res_b <= a_b;
				when "001" =>
					res <= b;
					res_b <= b_b;
				when "010" =>
					res <= c;
					res_b <= c_b;
				when "011" =>
					res <= d;
					res_b <= d_b;
				when "100" =>
					res <= e;
					res_b <= e_b;
				when "101" =>
					res <= f;
					res_b <= f_b;
				when "110" =>
					res <= g;
					res_b <= g_b;
				when "111" =>
					res <= h;
					res_b <= h_b;
			end case;
		elsif(Rising_edge(clk_05) and flag = '1') then
            res <= std_logic_vector(shift_left(unsigned(res), 1));
            flag <= '0';
		elsif(rising_edge(clk_05) and flag = '0') then
			if(unsigned(counter1) = res_b) then
				counter1 <= (others => '0');
                flag <= '1';
			else
				counter1 <= std_logic_vector(unsigned(counter1)+1);
				res <= std_logic_vector(shift_left(unsigned(res), 1));
			end if;
		end if;
	end process;
			
	
	led <= res(11);
	
end be;
```
				
				
			
		
		
