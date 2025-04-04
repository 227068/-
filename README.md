# Задание по дисциплине: "Поддержка и тестирование программных модулей  Аксёнова Татьяна Геннадьевна  Вариант 2"

# Разработка графического приложения, которое будет запрашивать баллы за каждое выполненное студентом задание, суммировать баллы и выводить результат (сумма набранных баллов и оценка по 5-тибалльной шкале).

Листинг кода страницы MainWindow:
<Window x:Class="Task_TK.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Task_TK"
        mc:Ignorable="d"
        Title="Оценки за экзамен" Height="350" Width="400"
        Background="#F0F0F0">
    <Grid Margin="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="0,0,0,5">
            <TextBlock Text="Database (Max 22):" Width="150" VerticalAlignment="Center"/>
            <TextBox x:Name="dbScoreTextBox" Width="50" Margin="5,0,0,0" Text="0"/>
        </StackPanel>

        <StackPanel Grid.Row="2" Orientation="Horizontal" Margin="0,0,0,5">
            <TextBlock Text="Software (Max 38):" Width="150" VerticalAlignment="Center"/>
            <TextBox x:Name="softwareScoreTextBox" Width="50" Margin="5,0,0,0" Text="0" />
        </StackPanel>

        <StackPanel Grid.Row="3" Orientation="Horizontal" Margin="0,0,0,10">
            <TextBlock Text="Maintenance (Max 20):" Width="150" VerticalAlignment="Center"/>
            <TextBox x:Name="maintenanceScoreTextBox" Width="50" Margin="5,0,0,0" Text="0" />
        </StackPanel>

        <Button Content="Calculate Grade" Click="CalculateButton_Click" Padding="10,5" Grid.Row="3" HorizontalAlignment="Left" Margin="10,33,0,-45" Width="109"/>
        <TextBlock x:Name="resultTextBlock" Grid.Row="5" Margin="0,10,0,0" Text="" TextWrapping="Wrap" FontWeight="Bold" />
    </Grid>
</Window>

Отобразится элемент:
![image](https://github.com/user-attachments/assets/e54bace3-acf0-42fb-a4de-bba9fd4ef72a)

Листинг кода страницы MainWindow.xaml.cs:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace Task_TK
{
    /// <summary>
    /// Логика взаимодействия для MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
        private void CalculateButton_Click(object sender, RoutedEventArgs e)
        {
            try
            {
                // Get the scores from the text boxes
                int dbScore = int.Parse(dbScoreTextBox.Text);
                int softwareScore = int.Parse(softwareScoreTextBox.Text);
                int maintenanceScore = int.Parse(maintenanceScoreTextBox.Text);

                // Validate the input
                if (!IsValidScore(dbScore, 22) || !IsValidScore(softwareScore, 38) || !IsValidScore(maintenanceScore, 20))
                {
                    MessageBox.Show("Invalid input. Please enter scores within the allowed ranges.", "Error", MessageBoxButton.OK, MessageBoxImage.Error);
                    return;
                }

                // Calculate the total score
                int totalScore = dbScore + softwareScore + maintenanceScore;

                // Determine the grade
                string grade = GetGrade(totalScore);

                // Display the result
                resultTextBlock.Text = $"Total Score: {totalScore}\nGrade: {grade}";
            }
            catch (FormatException)
            {
                MessageBox.Show("Invalid input. Please enter numeric values only.", "Error", MessageBoxButton.OK, MessageBoxImage.Error);
            }
            catch (Exception ex)
            {
                MessageBox.Show($"An unexpected error occurred: {ex.Message}", "Error", MessageBoxButton.OK, MessageBoxImage.Error);
            }
        }

        private bool IsValidScore(int score, int maxScore)
        {
            return score >= 0 && score <= maxScore;
        }

        private string GetGrade(int totalScore)
        {
            if (totalScore >= 56 && totalScore <= 80)
            {
                return "5 (Excellent)";
            }
            else if (totalScore >= 32 && totalScore <= 55)
            {
                return "4 (Good)";
            }
            else if (totalScore >= 16 && totalScore <= 31)
            {
                return "3 (Satisfactory)";
            }
            else
            {
                return "2 (Fail)";
            }
        }
    }
}

Попробуем запустить приложение и ввести данные:
![image](https://github.com/user-attachments/assets/fc13cda1-a25f-4bc7-b538-298be0b5dcbd)
![image](https://github.com/user-attachments/assets/79df9bc9-f6c7-4c55-af01-1f06f6d9ff54)

Теперь создадим позитивные и негативные автоматизированные тесты для тестирования разработанного приложения:
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;

namespace ExamGradesApp.Tests
{
    [TestClass]
    public class UnitTest1
    {
        [TestMethod]
        public void TestMethod1()
        {
            var mainWindow = new MainWindow(); // or a mock if needed
            int totalScore = 80;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("5 (Excellent)", grade);
        }

        [TestMethod]
        public void GetGrade_TotalScore56_ReturnsExcellent()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int totalScore = 56;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("5 (Excellent)", grade);
        }


        [TestMethod]
        public void GetGrade_TotalScore55_ReturnsGood()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int totalScore = 55;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("4 (Good)", grade);
        }

        [TestMethod]
        public void GetGrade_TotalScore32_ReturnsGood()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int totalScore = 32;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("4 (Good)", grade);
        }

        [TestMethod]
        public void GetGrade_TotalScore31_ReturnsSatisfactory()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int totalScore = 31;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("3 (Satisfactory)", grade);
        }

        [TestMethod]
        public void GetGrade_TotalScore16_ReturnsSatisfactory()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int totalScore = 16;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("3 (Satisfactory)", grade);
        }

        [TestMethod]
        public void GetGrade_TotalScore15_ReturnsFail()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int totalScore = 15;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("2 (Fail)", grade);
        }

        [TestMethod]
        public void GetGrade_TotalScore0_ReturnsFail()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int totalScore = 0;

            // Act
            string grade = mainWindow.GetGrade(totalScore);

            // Assert
            Assert.AreEqual("2 (Fail)", grade);
        }

        [TestMethod]
        public void IsValidScore_Score0Max22_ReturnsTrue()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int score = 0;
            int maxScore = 22;

            // Act
            bool isValid = mainWindow.IsValidScore(score, maxScore);

            // Assert
            Assert.IsTrue(isValid);
        }

        [TestMethod]
        public void IsValidScore_Score22Max22_ReturnsTrue()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int score = 22;
            int maxScore = 22;

            // Act
            bool isValid = mainWindow.IsValidScore(score, maxScore);

            // Assert
            Assert.IsTrue(isValid);
        }

        [TestMethod]
        public void IsValidScore_Score23Max22_ReturnsFalse()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int score = 23;
            int maxScore = 22;

            // Act
            bool isValid = mainWindow.IsValidScore(score, maxScore);

            // Assert
            Assert.IsFalse(isValid);
        }

        [TestMethod]
        public void IsValidScore_ScoreNegative1Max22_ReturnsFalse()
        {
            // Arrange
            var mainWindow = new MainWindow();
            int score = -1;
            int maxScore = 22;

            // Act
            bool isValid = mainWindow.IsValidScore(score, maxScore);

            // Assert
            Assert.IsFalse(isValid);
        }
    }
}
![image](https://github.com/user-attachments/assets/0a720c05-72ac-4a2f-b90f-e2001e6d06f6)





