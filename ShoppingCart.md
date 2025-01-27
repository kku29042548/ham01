using System.Diagnostics.Eventing.Reader;
using System.Drawing;
using System.Security.Policy;
using static System.Net.WebRequestMethods;
using static System.Windows.Forms.LinkLabel;

namespace WinFormsApp1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        public double CalculateDrinkTotal(TextBox coffeePriceTextBox, TextBox coffeeAmountTextBox, TextBox teaPriceTextBox, TextBox teaAmountTextBox, CheckBox coffeeCheckBox, CheckBox teaCheckBox)
        {
            string coffeePriceInput = coffeePriceTextBox.Text;
            string coffeeAmountInput = coffeeAmountTextBox.Text;
            string teaPriceInput = teaPriceTextBox.Text;
            string teaAmountInput = teaAmountTextBox.Text;

            int coffeePrice = 0;
            int coffeeAmount = 0;
            int teaAmount = 0;
            int teaPrice = 0;

            try
            {
                if (coffeeCheckBox.Checked)
                    coffeePrice = int.Parse(coffeePriceInput);
                coffeeAmount = int.Parse(coffeeAmountInput);
            }
            catch (FormatException) { }
            try
            {
                if (teaCheckBox.Checked)
                    teaPrice = int.Parse(teaPriceInput);
                teaAmount = int.Parse(teaAmountInput);
            }
            catch (FormatException)
            {

            }
            double coffeeTotal = coffeePrice * coffeeAmount;
            double teaTotal = teaPrice * teaAmount;
            double total = coffeeTotal + teaTotal;
            return total;
        }
        public double ApplyTotalDiscount(double drinkTotal, double foodTotal)
        {
            double discountedTotal = 0;
            if (Discount_All.Checked)
            {
                double discountValue = 0;
                try
                {
                    discountValue = int.Parse(Tb_DisAll.Text);
                    double combinedTotal = drinkTotal + foodTotal;
                    combinedTotal -= (combinedTotal * discountValue / 100);
                    discountedTotal += combinedTotal;

                }
                catch (FormatException)
                {
                    double combinedTotal = drinkTotal + foodTotal;
                    discountedTotal += combinedTotal;
                }


            }
            return discountedTotal;
        }
        public double ApplyDrinkDiscount(double drinkTotal)
        {
            if (Discount_Drink.Checked)
            {
                double discountValue = 0;
                try
                {
                    discountValue = int.Parse(Tb_DisDrink.Text);
                    drinkTotal -= (drinkTotal * discountValue / 100);

                }
                catch (FormatException)
                { drinkTotal -= (drinkTotal * discountValue / 100); }

            }
            return drinkTotal;
        }
        public double ApplyFoodDiscount(double foodTotal)
        {
            if (Discount_Food.Checked)
            {
                double discountValue = 0;
                try
                {
                    discountValue = int.Parse(Tb_DisFood.Text);
                    foodTotal -= (foodTotal * discountValue / 100);

                }
                catch (FormatException)
                {
                    foodTotal -= (foodTotal * discountValue / 100);

                }
            }

            return foodTotal;
        }


        private void textBox3_TextChanged(object sender, EventArgs e)
        {

        }

        private void checkBox1_CheckedChanged(object sender, EventArgs e)
        {
            if (Cola_Check.Checked == false)
            {
                ColaPrice.Enabled = false;
                ColaAmount.Enabled = false;
            }
            if (Cola_Check.Checked == true)
            {
                ColaPrice.Enabled = true;
                ColaAmount.Enabled = true;
            }

        }

        private void CoffeePrice_TextChanged(object sender, EventArgs e)
        {

        }

        private void button1_Click(object sender, EventArgs e)
        {
            double drinkTotal = CalculateDrinkTotal(ColaPrice, ColaAmount, WaterPrice, WaterAmount, Cola_Check, water_Check);
            double foodTotal = CalculateDrinkTotal(Canfish_Price, Canfish_Amount, Noondle_price, Noondle_Amount, Canfish_Check, Noondle_Check);
            double finalTotal = 0;
            if (Discount_All.Checked)
            {
                finalTotal += ApplyTotalDiscount(drinkTotal, foodTotal);

            }
            else if (Discount_Drink.Checked)
            {
                drinkTotal = ApplyDrinkDiscount(drinkTotal);
                finalTotal += drinkTotal + foodTotal;

            }

            else if (Discount_Food.Checked)
            {
                foodTotal = ApplyFoodDiscount(foodTotal);
                finalTotal += foodTotal + drinkTotal;



            }
            else
            {
                finalTotal += drinkTotal + foodTotal;
            }
            Total.Text = finalTotal.ToString();



        }

        private void button2_Click(object sender, EventArgs e)
        {
            double total = 0;
            double cash = 0;
            try
            {

                cash = double.Parse(Cash.Text);

            }
            catch (FormatException) { }

            try
            {
                total = double.Parse(Total.Text);
            }
            catch (FormatException) { }
            double change = cash - total;
            Changebox.Text = change.ToString();

            double OneT = 0;
            double FiveH = 0;
            double oneH = 0;
            double fifty = 0;
            double twenty = 0;
            double ten = 0;
            double five = 0;
            double one = 0;
            double fiftystang = 0;
            double twentyfivestang = 0;
            double tenstang = 0;
            double fivestang = 0;
            double onestang = 0;
            while (change >= 0.01)
            {
                if (change >= 1000)
                {
                    change -= 1000;
                    OneT++;
                }
                else if (change >= 500)
                {
                    change -= 500;
                    FiveH++;
                }
                else if (change >= 100)
                {
                    change -= 100;
                    oneH++;
                }
                else if (change >= 50)
                {
                    change -= 50;
                    fifty++;
                }
                else if (change >= 20)
                {
                    change -= 20;
                    twenty++;
                }
                else if (change >= 10)
                {
                    change -= 10;
                    ten++;
                }
                else if (change >= 5)
                {
                    change -= 5;
                    five++;
                }
                else if (change >= 1)
                {
                    change -= 1;
                    one++;
                }
                else if (change >= 0.50)
                {
                    change -= 0.50;
                    fiftystang++;
                }
                else if (change >= 0.25)
                {
                    change -= 0.25;
                    twentyfivestang++;
                }
                else if (change >= 0.10)
                {
                    change -= 0.10;
                    tenstang++;
                }
                else if (change >= 0.05)
                {
                    change -= 0.05;
                    fivestang++;

                }
                else if (change >= 0.01)
                {
                    change -= 0.01;
                    onestang++;
                }
            }
            OneThousand.Text = OneT.ToString();
            FiveHundred.Text = FiveH.ToString();
            OneHundred.Text = oneH.ToString();
            Fifty.Text = fifty.ToString();
            Twenty.Text = twenty.ToString();
            Ten.Text = ten.ToString();
            Five.Text = five.ToString();
            One.Text = one.ToString();
            FiftyStang.Text = fiftystang.ToString();
            TwentyFiveStang.Text = twentyfivestang.ToString();
            TenStang.Text = tenstang.ToString();
            FiveStang.Text = fivestang.ToString();
            OneStang.Text = onestang.ToString();


        }

        private void Tea_check_CheckedChanged(object sender, EventArgs e)
        {
            if (water_Check.Checked == true)
            {
                WaterPrice.Enabled = true;
                WaterAmount.Enabled = true;
            }
            if (water_Check.Checked == false)
            {
                WaterPrice.Enabled = false;
                WaterAmount.Enabled = false;
            }
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void groupBox1_Enter(object sender, EventArgs e)
        {

        }

        private void groupBox4_Enter(object sender, EventArgs e)
        {

        }

        private void label17_Click(object sender, EventArgs e)
        {

        }

        private void checkBox3_CheckedChanged(object sender, EventArgs e)
        {
            if (Discount_Drink.Checked)
            {
                Discount_All.Checked = false;
                Discount_Food.Checked = false;
                Tb_DisDrink.Enabled = true;
            }
            else
            {
                Tb_DisDrink.Enabled = false;
            }
        }

        private void Discount_Food_CheckedChanged(object sender, EventArgs e)
        {
            if (Discount_Food.Checked)
            {
                Discount_All.Checked = false;
                Discount_Drink.Checked = false;
                Tb_DisFood.Enabled = true;
            }
            else
            {
                Tb_DisFood.Enabled = false;
            }
        }

        private void Discount_All_CheckedChanged(object sender, EventArgs e)
        {
            if (Discount_All.Checked)
            {
                Discount_Drink.Checked = false;
                Discount_Food.Checked = false;
                Discount_All.Checked = true;
                Tb_DisAll.Enabled = true;
            }
            else
            {
                Tb_DisAll.Enabled = false;
            }

        }


        private void Pizza_Check_CheckedChanged(object sender, EventArgs e)
        {
            if (Canfish_Check.Checked)
            {
                Canfish_Price.Enabled = true;
                Canfish_Amount.Enabled = true;
            }
            else
            {
                Canfish_Price.Enabled = false;
                Canfish_Amount.Enabled = false;
            }

        }

        private void Burger_Check_CheckedChanged(object sender, EventArgs e)
        {
            if (Noondle_Check.Checked == true)
            {
                Noondle_price.Enabled = true;
                Noondle_Amount.Enabled = true;
            }
            else
            {
                Noondle_price.Enabled = false;
                Noondle_Amount.Enabled = false;
            }

        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {

        }

        private void One_TextChanged(object sender, EventArgs e)
        {

        }

        private void Fifty_TextChanged(object sender, EventArgs e)
        {

        }
    }
}
