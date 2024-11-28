package com.yujin.jdbc.view;

import java.awt.Component;
import java.awt.EventQueue;

import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.border.EmptyBorder;

import com.yujin.jdbc.controller.StudentDao;
import com.yujin.jdbc.model.Student;

import javax.swing.JLabel;
import javax.swing.JOptionPane;

import java.awt.Font;
import javax.swing.SwingConstants;
import javax.swing.JTextField;
import javax.swing.JButton;
import java.awt.event.ActionListener;
import java.sql.Clob;
import java.awt.event.ActionEvent;

public class StudentInfoFrame extends JFrame {
	
	public interface CreateNotify { 
		void notifyCreateSuccess(); //생성 성공했다는 걸 알려주는 메서드.
	}
	
	private static final long serialVersionUID = 1L;
	private JPanel contentPane;
	private JLabel idLabel;
	private JPanel panel;
	private JTextField idTextField;
	private JTextField gradeTextField;
	private JTextField classTextField;
	private JTextField nametextField;
	private JLabel gradeLabel;
	private JLabel classLabel;
	private JLabel nameLabel;
	private JPanel memo_contact_panel;
	private JLabel memoLable;
	private JTextField memoTextField;
	private JLabel contactLable;
	private JTextField contactTextField;
	private JPanel buttonPanel;
	private JButton saveButton;
	private JButton cancelButton;
	
	private StudentDao studentDao; //iv)
	private Component parentComponent; //
	private CreateNotify app; //v)

	/**
	 * Launch the application.
	 */
	public static void showStudentInfoFrame(Component parentComponent, CreateNotify app) { //
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					StudentInfoFrame frame = new StudentInfoFrame(parentComponent, app); //
					frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	//생성자
	private StudentInfoFrame(Component parentComponent, CreateNotify app) { //
		this.studentDao = StudentDao.INSTANCE;
		this.parentComponent = parentComponent;
		this.app = app;
		initialize();
	}
	/**
	 *UI 컴포넌트들을 초기화. //
	 */
	public void initialize() { //
		// JFrame의 타이틀 설정.
		setTitle("학생 정보 입력"); //
		
		setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE); //
		setBounds(100, 100, 363, 479);
		
		//부모 컴포넌트의 가운데 위치에 창을 뛰움.
		//부모 컴포넌트가 null이면 화면의 가운데 위치에 창을 띄움.
		setLocationRelativeTo(parentComponent); //
		
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));

		setContentPane(contentPane);
		contentPane.setLayout(null);
		
		panel = new JPanel();
		panel.setBounds(10, 10, 331, 153);
		contentPane.add(panel);
		panel.setLayout(null);
		
		idLabel = new JLabel("ID");
		idLabel.setHorizontalAlignment(SwingConstants.CENTER);
		idLabel.setFont(new Font("D2Coding", Font.BOLD, 20));
		idLabel.setBounds(24, 10, 50, 38);
		panel.add(idLabel);
		
		idTextField = new JTextField();
		idTextField.setFont(new Font("D2Coding", Font.PLAIN, 20));
		idTextField.setBounds(81, 10, 139, 38);
		panel.add(idTextField);
		idTextField.setColumns(10);
		
		gradeLabel = new JLabel("학년");
		gradeLabel.setHorizontalAlignment(SwingConstants.CENTER);
		gradeLabel.setFont(new Font("D2Coding", Font.BOLD, 20));
		gradeLabel.setBounds(69, 58, 50, 38);
		panel.add(gradeLabel);
		
		gradeTextField = new JTextField();
		gradeTextField.setFont(new Font("D2Coding", Font.PLAIN, 20));
		gradeTextField.setColumns(10);
		gradeTextField.setBounds(24, 58, 44, 38);
		panel.add(gradeTextField);
		
		classLabel = new JLabel("반");
		classLabel.setHorizontalAlignment(SwingConstants.CENTER);
		classLabel.setFont(new Font("D2Coding", Font.BOLD, 20));
		classLabel.setBounds(179, 58, 41, 38);
		panel.add(classLabel);
		
		classTextField = new JTextField();
		classTextField.setFont(new Font("D2Coding", Font.PLAIN, 20));
		classTextField.setColumns(10);
		classTextField.setBounds(129, 58, 50, 38);
		panel.add(classTextField);
		
		nameLabel = new JLabel("이름");
		nameLabel.setHorizontalAlignment(SwingConstants.CENTER);
		nameLabel.setFont(new Font("D2Coding", Font.BOLD, 20));
		nameLabel.setBounds(21, 106, 50, 38);
		panel.add(nameLabel);
		
		nametextField = new JTextField();
		nametextField.setFont(new Font("D2Coding", Font.PLAIN, 20));
		nametextField.setColumns(10);
		nametextField.setBounds(81, 106, 139, 38);
		panel.add(nametextField);
		
		memo_contact_panel = new JPanel();
		memo_contact_panel.setBounds(10, 173, 331, 212);
		contentPane.add(memo_contact_panel);
		memo_contact_panel.setLayout(null);
		
		memoLable = new JLabel("메모란");
		memoLable.setHorizontalAlignment(SwingConstants.CENTER);
		memoLable.setFont(new Font("D2Coding", Font.BOLD, 20));
		memoLable.setBounds(22, 0, 60, 38);
		memo_contact_panel.add(memoLable);
		
		memoTextField = new JTextField();
		memoTextField.setHorizontalAlignment(SwingConstants.LEFT);
		memoTextField.setFont(new Font("D2Coding", Font.PLAIN, 20));
		memoTextField.setBounds(22, 38, 288, 93);
		memo_contact_panel.add(memoTextField);
		memoTextField.setColumns(10);
		
		contactLable = new JLabel("연락처");
		contactLable.setHorizontalAlignment(SwingConstants.CENTER);
		contactLable.setFont(new Font("D2Coding", Font.BOLD, 20));
		contactLable.setBounds(22, 130, 60, 38);
		memo_contact_panel.add(contactLable);
		
		contactTextField = new JTextField();
		contactTextField.setFont(new Font("D2Coding", Font.PLAIN, 20));
		contactTextField.setColumns(10);
		contactTextField.setBounds(22, 164, 288, 38);
		memo_contact_panel.add(contactTextField);
		
		buttonPanel = new JPanel();
		buttonPanel.setBounds(10, 395, 331, 39);
		contentPane.add(buttonPanel);
		
		saveButton = new JButton("저장");
		saveButton.addActionListener(e -> insertNewInfo());
		saveButton.setFont(new Font("D2Coding", Font.BOLD, 20));
		buttonPanel.add(saveButton);
		
		cancelButton = new JButton("취소");
		cancelButton.addActionListener(e -> dispose());
		cancelButton.setFont(new Font("D2Coding", Font.BOLD, 20));
		buttonPanel.add(cancelButton);
	}

	private void insertNewInfo() {
		//입력된 메모란, 연락처를 읽음
		String memo = memoTextField.getText();
		String contact = contactTextField.getText();
		if(memo.equals("") || contact.equals("")) {
			JOptionPane.showMessageDialog(
    				StudentInfoFrame.this, //부모컴포넌트의 인스턴스
    				"제목과 내용은 반드시 입력해야 합니다.", 
    				"경고", 
    				JOptionPane.WARNING_MESSAGE);
    		return;
		}
		
		Student student = Student.builder().memo(memo).contact(contact).build();
		int result = studentDao.insertNewInfo(student);
		
	}

}
