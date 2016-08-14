using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace AAA
{
    public partial class Form1 : Form
    {
        string str1, str2, str4, str5, str6;
        string s1, s2, s3, s4, s5;
        bool w1, w2;
        int a, b, c;
        private void button1_Click_1(object sender, EventArgs e)
        {
            label3.Text = "请重新载入文件！";
            label2.Text = "";
            DialogResult hry;
            hry = MessageBox.Show("请重新载入文件！", "提示");
            s1 = "";
            str4 = "";
            w1 = false;
            w2 = false;
            Flash.Checked = true;
            APP1.Checked = true;
            textBox1.Text = "02000400";
        }
        private void textBox1_TextChanged(object sender, EventArgs e)
        {
        }
        private void APP1_CheckedChanged(object sender, EventArgs e)
        {
            str2 = ":044C04000050000854\r\n";
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            Flash.Checked = true;
            APP1.Checked = true;
        }

        private void APP2_CheckedChanged(object sender, EventArgs e)
        {
            str2 = ":044C0400001801088B\r\n";
        }

        private void JointFile2_Click(object sender, EventArgs e)
        {
           
                OpenFileDialog ofdg2 = new OpenFileDialog();
                ofdg2.Filter = "hex files (*.hex)|*.hex|All files (*.*)|*.*";//设置打开文件类型
                if (ofdg2.ShowDialog(this) == System.Windows.Forms.DialogResult.OK)
                {
                    string file = ofdg2.FileName;//得到选择的文件的完整路径
                    str4 = System.IO.File.ReadAllText(file, Encoding.Default);
                    c = str4.LastIndexOf(":00000001FF");
                    str5 = str4.Substring(16, c - 16);
                    string result;
                    result = System.IO.Path.GetFileNameWithoutExtension(ofdg2.FileName);
                    String subStr1 = "APP1";
                    String subStr2 = "APP2";
                    if (result.Contains(subStr1))
                    {
                        label2.Text = "将载入固件：\r\nAPP1固件";
                        w2 = true;
                    }
                    else if (result.Contains(subStr2))
                    {
                        label2.Text = "将载入固件：\r\nAPP2固件";
                        w2 = true;
                    }
                    else
                    {
                        DialogResult dr;
                        dr = MessageBox.Show("载入文件有误，请重新加载APP1或APP2固件", "提示");
                        label2.Text = "";
                    }
                }
        }
        private void RAM_CheckedChanged(object sender, EventArgs e)
        {
            str1 = ":044C000003000000AD\r\n";
        }
        private void Flash_CheckedChanged(object sender, EventArgs e)
        {
            str1 = ":044C000002000000AE\r\n";
        }

        private void Save_Click(object sender, EventArgs e)
        {
            string str = textBox1.Text.Trim();//获取用户输入的字符串
            if (IsNumber(str))
            {
               
                    if ((w1 && w2))
                    {
                        s2 = s1.Insert(a, str1);
                        a = s2.IndexOf(":00000001FF");
                        s3 = s2.Insert(a, str2);
                        a = s3.IndexOf(":00000001FF");
                        int x1 = int.Parse(textBox1.Text.Substring(0, 1));
                        int x2 = int.Parse(textBox1.Text.Substring(1, 1));
                        int x3 = int.Parse(textBox1.Text.Substring(2, 1));
                        int x4 = int.Parse(textBox1.Text.Substring(3, 1));
                        int x5 = int.Parse(textBox1.Text.Substring(4, 1));
                        int x6 = int.Parse(textBox1.Text.Substring(5, 1));
                        int x7 = int.Parse(textBox1.Text.Substring(6, 1));
                        int x8 = int.Parse(textBox1.Text.Substring(7, 1));
                        int m1 = x1 * 16 + x2;
                        int m2 = x3 * 16 + x4;
                        int m3 = x5 * 16 + x6;
                        int m4 = x7 * 16 + x8;
                        int sum = 256 - (96 + m1 + m2 + m3 + m4);
                        string ss = Convert.ToString(sum, 16);
                        string so = ss.Substring(ss.Length - 2, 2);
                        str4 = ":044C1000" + textBox1.Text + so + "\r\n";
                        s4 = s3.Insert(a, str4);
                        a = s4.IndexOf(":00000001FF");
                        str6 = s4.Insert(a, str5);
                        b = str6.LastIndexOf(":00000001FF") - 1;
                        s5 = str6.Substring(0, b + 14);
                        System.IO.StreamWriter myStream;
                        saveFileDialog1.Filter = "hex files (*.hex)|*.hex|All files (*.*)|*.*";
                        saveFileDialog1.FilterIndex = 2;
                        saveFileDialog1.RestoreDirectory = true;
                        if (saveFileDialog1.ShowDialog() == DialogResult.OK)
                        {
                            myStream = new System.IO.StreamWriter(saveFileDialog1.FileName);
                            myStream.Write(s5); //写入
                            myStream.Close();//关闭流
                        }
                    }
                    else
                    {
                        DialogResult bry;
                        bry = MessageBox.Show("文件不完整，请重新选择文件", "提示");
                    }
                    label2.Text = "";
                    label3.Text = "";         
            }
            else
            {
                DialogResult ad;
                ad = MessageBox.Show("Hw Version有误，请重新输入", "提示");
            }
                
        }
        private void Bootloader_CheckedChanged(object sender, EventArgs e)
        {
             str1 = ":044C000001000000AF\r\n";
           
        }
        public Form1()
        {
            InitializeComponent();
            textBox1.Text = "02000400";
            str1 = ":044C000001000000AF\r\n";
            str2 = ":044C04000050000854\r\n";
        }
        private void button1_Click(object sender, EventArgs e)
        {
            OpenFileDialog ofdg1 = new OpenFileDialog();
            ofdg1.Filter = "hex files (*.hex)|*.hex|All files (*.*)|*.*";//设置打开文件类型
            if (ofdg1.ShowDialog(this) == System.Windows.Forms.DialogResult.OK)
            {
                string file = ofdg1.FileName;//得到选择的文件的完整路径
                s1 = System.IO.File.ReadAllText(file, Encoding.Default);//把读出来的数据显示在textbox中
                a = s1.IndexOf(":00000001FF");
                w1 = true ;
                label3.Text = "Bootloader载入成功!";
            }
        }
        //设置判断Hw Version是否为数字的方法
        public static bool IsNumber(string str)
        {
            if (str == null || str.Length != 8)
                return false;
            ASCIIEncoding ascii = new ASCIIEncoding();//new ASCIIEncoding 的实例
            byte[] bytestr = ascii.GetBytes(str);//把string类型的参数保存到数组里

            foreach (byte c in bytestr)   //遍历这个数组里的内容
            {
                if (c < 48 || c > 57)
                {
                    return false;
                }
            }
            return true;
        }
    }
}
