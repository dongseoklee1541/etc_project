```JAVA
package project2017;

import java.lang.*;
import javax.swing.*;
import java.awt.event.*;
import java.awt.*;
import java.io.*;
import java.util.Calendar;


public class project extends JFrame {
	Container contentPane;
	int [] data = {0,0,0,0};
	int [] arcAngle = new int [4];
	
	Calendar today = Calendar.getInstance();
	int calYear = today.get(Calendar.YEAR); 
	int calMonth = today.get(Calendar.MONTH);
	int calDayOfMon = today.get(Calendar.DAY_OF_MONTH); // 오늘 날짜
	String calMonth1 = Integer.toString(calMonth+1); 
	String calday1 = Integer.toString(calDayOfMon); // string으로 변경
	
	Color [] color = {Color.RED, Color.BLUE, Color.MAGENTA, Color.ORANGE};
	String [] itemName = {"밥", "술", "군것질", "음료"}; // 식단 목록
	JTextField [] tf = new JTextField [4]; // 섭취횟수
	ChartPanel chartPanel = new ChartPanel();
	
	JTextArea cmt = new JTextArea(10,10); // 코멘트
	

	JLabel month1 = new JLabel("월 ");
	JLabel day1 = new JLabel("일 ");
	JTextField days0 = new JTextField(calMonth1, 2);
	JTextField days1 = new JTextField(calday1, 2);
	






	project() 
	{
		setTitle("식단관리");
		

		
		createMenu();
		// 오늘 월일날짜 넣기




		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		contentPane = getContentPane();
		contentPane.add(new InputPanel(), BorderLayout.CENTER);
		contentPane.add(chartPanel, BorderLayout.NORTH);
		contentPane.add(new MyPanel(),BorderLayout.WEST);
		contentPane.add(cmt, BorderLayout.SOUTH);
		

		

			
		
		setVisible(true);
		drawChart();
		
	
	}
	
		
	void createMenu() // 메뉴바 만드는 메소드
	{
		JMenuBar mb = new JMenuBar(); 
		JMenu filemenu = new JMenu("파일관리");
		
		JMenuItem  [] menuItem = new JMenuItem [2];
		String[] itemTitle = {"SAVE", "DELETE"};
		
		for(int i=0;i<menuItem.length;i++)
		{
			menuItem[i] = new JMenuItem(itemTitle[i]); 
			menuItem[i].addActionListener(new MenuActionListener());
			filemenu.add(menuItem[i]);
		}
		mb.add(filemenu);
		setJMenuBar(mb);
		
	}
	
	class MenuActionListener implements ActionListener // 메뉴바 이벤트 리스너
	{
		
		public void actionPerformed(ActionEvent arg0) {
			
			String monthstr = days0.getText();
			String daystr = days1.getText();
			int monval = Integer.parseInt(monthstr);
			int dayval = Integer.parseInt(daystr); 
			
			String cmd = arg0.getActionCommand();
			switch(cmd)
			{
			case "SAVE":
			try {
				
				File f= new File("MemoData");
				if(!f.isDirectory()) f.mkdir();
				
				String memo = cmt.getText();
				if(memo.length()>0){
					BufferedWriter out = new BufferedWriter(new FileWriter("MemoData/"+(calYear)+((monval)<10?"0":"")+(monval)+(dayval<10?"0":"")+dayval+".txt"));
					String str = cmt.getText();
					out.write(str);  
					out.close();
					JOptionPane.showMessageDialog(null,
							"저장 되었습니다.", "Message",
							JOptionPane.ERROR_MESSAGE);
				}
				else
				{
					JOptionPane.showMessageDialog(null,
							"메모를 우선 작성하세요.", "Message",
							JOptionPane.ERROR_MESSAGE);
				}
			}
			catch (IOException e) {
				JOptionPane.showMessageDialog(null,
						"파일 쓰기 실패", "Message",
						JOptionPane.ERROR_MESSAGE);

			}//end of catch
			break;
			case "DELETE":
				cmt.setText("");
				File f =new File("MemoData/"+(calYear)+((monval)<10?"0":"")+(monval)+(dayval<10?"0":"")+dayval+".txt");
				if(f.exists()){
					f.delete();
					JOptionPane.showMessageDialog(null,
							"메모를 삭제하였습니다.", "Message",
							JOptionPane.ERROR_MESSAGE);
				}
				else 
					JOptionPane.showMessageDialog(null,
							"작성되지 않았거나 이미 삭제된 메모입니다.", "Message",
							JOptionPane.ERROR_MESSAGE);		
			}	
		}
		
	} // end of MenuActionListener
	
	void drawChart() { // 차트 그리기위한 데이터 입력
		int sum=0;
		for(int i=0; i<data.length; i++) {
			data[i] = Integer.parseInt(tf[i].getText());
			sum+=data[i];			
		}

		if(sum == 0) return;

		for(int i=0; i<data.length; i++) {
			arcAngle[i]=(int)Math.round((double)data[i]/(double)sum*360);
		}
		save();
		chartPanel.repaint();
	}

	
	void save()
		{
			String monthstr = days0.getText();
			String daystr = days1.getText();
			int monval = Integer.parseInt(monthstr);
			int dayval = Integer.parseInt(daystr); 
			try
			{
			   PrintWriter  fw = new PrintWriter("MemoData/"+(calYear)+((monval)<10?"0":"")+(monval)+(dayval<10?"0":"")+dayval+"num.txt");
	        for(int i=0; i<data.length; i++) 
	        {
	            String data1 = Integer.toString(data[i]);
	            fw.println(data1);
	        }
	        fw.close();
	      }
			catch(IOException e)
			{
				e.printStackTrace();
			}
		}


	
	class MyPanel extends JPanel{// 서쪽 월 일 표시

		MyPanel()
		{
			add(days0);
			add(month1);
			add(days1);
			add(day1);

			days0.addKeyListener(new NumberField());
			days1.addKeyListener(new NumberField());
			days0.addActionListener(new enterField());
			days1.addActionListener(new enterField());
		}
		
		
	}
	

	public class NumberField extends JTextField implements KeyListener {// 텍스트필드에 숫자만 넣게 하기
		 private static final long serialVersionUID = 1;
		 
		 public NumberField() {
		  addKeyListener(this);
		 }

		 public void keyPressed(KeyEvent e) {
		 }

		 public void keyReleased(KeyEvent e) {
		 }

		 public void keyTyped(KeyEvent e) {
		  // Get the current character you typed...
		  char c = e.getKeyChar();
		  
		  if (!Character.isDigit(c)) {
		   e.consume();
		   return;
		  }
		 }
		}
	public class enterField implements ActionListener// 엔터 입력시 이전에 있는 메모 소환
	{
		public void actionPerformed(ActionEvent arg0) {
			
			String monthstr = days0.getText();
			String daystr = days1.getText();
			int monval = Integer.parseInt(monthstr);
			int dayval = Integer.parseInt(daystr); 
			try{// 날짜에 맞춰 메모 읽어오기
				File f = new File("MemoData/"+(calYear)+((monval)<10?"0":"")+(monval)+(dayval<10?"0":"")+dayval+".txt");
				if(f.exists()){
					BufferedReader in = new BufferedReader(new FileReader("MemoData/"+(calYear)+((monval)<10?"0":"")+(monval)+(dayval<10?"0":"")+dayval+".txt"));
					String memoAreaText= new String();
					while(true){
						String tempStr = in.readLine();
						if(tempStr == null) break;
						memoAreaText = memoAreaText + tempStr + System.getProperty("line.separator");
					}
					cmt.setText(memoAreaText);
					
					in.close();	
				}
				else cmt.setText("");
			} catch (IOException e) {
				e.printStackTrace();
			}
			try{// 날짜에 맞춰 횟수 가져오기
		        BufferedReader br = new BufferedReader(new FileReader("MemoData/"+(calYear)+((monval)<10?"0":"")+(monval)+(dayval<10?"0":"")+dayval+"num.txt"));
		        
		        for(int i=0; i<tf.length; i++) {
		            tf[i].setText(br.readLine());
					
		        }
		        drawChart();
		        br.close();
		    }catch (FileNotFoundException e) // 파일이 없는 경우 횟수 초기화
			{
				for(int i=0; i<tf.length; i++) 
				{
					tf[i] .setText("0");
				}
				drawChart();
				e.printStackTrace();
			}
			catch (IOException e) 
			{
		    	
			e.printStackTrace();
			}
			
		}
		
	}// end of enterfiled
	
	

	class InputPanel extends JPanel {  // 섭취 횟수 입력패널
		InputPanel() { 
			this.setBackground(Color.LIGHT_GRAY);
			String monthstr = days0.getText();
			String daystr = days1.getText();
			int monval = Integer.parseInt(monthstr);
			int dayval = Integer.parseInt(daystr); 
			add(new JLabel("섭취횟수: "));
			
			try{
		        BufferedReader br = new BufferedReader(new FileReader("MemoData/"+(calYear)+((monval)<10?"0":"")+(monval)+(dayval<10?"0":"")+dayval+"num.txt"));
		        
		        for(int i=0; i<tf.length; i++) {
		            tf[i] = new JTextField(br.readLine(),5);
					tf[i].addActionListener(new MyActionListener());
					tf[i].addKeyListener(new NumberField());
					add(new JLabel(itemName[i])); // 메뉴 삽입
					add(tf[i]);

		        }
		        br.close();
		    }catch (FileNotFoundException e) 
			{
				for(int i=0; i<tf.length; i++) {
					tf[i] = new JTextField("0", 5);
					tf[i].addActionListener(new MyActionListener());
					tf[i].addKeyListener(new NumberField());
					add(new JLabel(itemName[i])); // 메뉴 삽입
					add(tf[i]);
			e.printStackTrace();
			}
			}
			catch (IOException e) 
			{
		    	
			e.printStackTrace();
			}

		}
		class MyActionListener implements ActionListener {
			public void actionPerformed(ActionEvent e) {
				JTextField t = (JTextField)e.getSource();
				int n;
				try {
					n = Integer.parseInt(t.getText());
				}catch(NumberFormatException ex) {
					t.setText("0");
					return;
				}
				
				drawChart();
			}

		}//end of MyActionListener

}//end of inputpanel
	

	class ChartPanel extends JPanel {
		ChartPanel()
		{
			
			this.setPreferredSize(new Dimension(200,320));
			
		}
		public void paintComponent(Graphics g) {
			super.paintComponent(g);
			g.drawString(0 + "", 25, 255);
			g.drawLine(50, 250, 450, 250);
			g.drawLine(50, 30, 50, 250); 
            for (int cnt = 1; cnt < 11; cnt++) {
                g.drawString(cnt*1 + "", 25, 255 - 20*cnt);  
                g.drawLine(50, 250 - 20*cnt, 450, 250 - 20*cnt);
            } // 배경에 표기선
            
            for(int i=0; i<data.length;i++)
            {
            	
            	 g.drawString(itemName[i], 100 + i*100, 270);
            } // 차트 바로 밑에 표기
            
 
			
			for(int i=0; i<data.length; i++) {
				g.setColor(color[i]);
				g.drawString(itemName[i]+" "+Math.round(arcAngle[i]*100./360.)+"%", 90+i*100, 310);
			} // 식단과 퍼센트
			for(int i=0; i<data.length; i++) {
				g.setColor(color[i]);
				g.fillRect(110 + i*100, 250 - data[i]*20 ,10,data[i]*20);
				
			} // 차트 그리기
		}	
		
	}
	

	public static void main(String [] args) {
		project jr =  new project();
		jr.pack();
	} // end of main
}  // end of class

```
