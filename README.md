# Animal-Race
/*
Kelsey Acosta
October 27, 2023
Animal Race: has 3 animals that the user inputs race each other and the first to reach 100 wins.
 */

import java.util.Scanner;
import java.util.Random;
import java.awt.Toolkit;

//main code
public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        //arrays for animals and the letter that represents the animal
        String[] animals = new String[3];
        char[] letter = new char[3];

        //array for starting position for animals
        int[] position =  {1,1,1};
        char first;
        String Winner = " ";


            //loop to populate the array with animals (user input)
            for (int i = 0; i < 3; i++) {
                System.out.println("Insert Animal " + (i + 1) + ": ");
                animals[i] = input.next();
                //storing the  1st char of the word
                letter[i] = animals[i].charAt(0);
            }

            //checking that no two animals are represented by the same char
            if (letter[0] == letter[1]) {
                letter[1] = animals[1].charAt(1);

            }
            if (letter[0] == letter[2]) {
                letter[2] = animals[2].charAt(1);

            }
            if (letter[1] == letter[2]) {
                letter[2] = animals[2].charAt(2);
            }

            //start the race!!
            System.out.println("""
                     THREE
                     TWO
                     ONE
                     BANG!!
                     Let the Race...BEGIN!!!!\
                    """);

            //method to conduct race
            first = movement(position, letter, animals);

            for (int i = 0; i < letter.length; i++) {
                if (first == letter[i]) {
                    Winner = animals[i];
                }
            }

            //beep to announce the winner
            Toolkit.getDefaultToolkit().beep();

            //outputs race results
            System.out.println("""
                    \t \t \t \t RESULTS:
                                    
                    \t \t \t \tWOOHOOO!!!
                    \t \tTHE WINNER OF THE RACE IS....
                    """ + "\t" + "\t" + "\t" + "\t " + Winner + "!!!");
    }

    //method to output the track
    public static char movement(int counter[],char runner[], String competitor []){
        //control variable for finding the winner
        boolean status = false;
        char winner = ' ';
        int[] mini_counter = new int[2];
        String[] mini_competitors = new String[2];

        do {
            //loop to output the animals position and the track (once)
            for (int i = 0; i < 100; i++) {
                if (counter[0] == (i + 1)) {
                    System.out.print(runner[0]);
                }if (counter[1] == (i + 1)) {
                    System.out.print(runner[1]);
                }if (counter[2] == (i + 1)) {
                    System.out.print(runner[2]);
                } else {
                    System.out.print("_");
                }
            }
            System.out.println();

            //loop to verify if there is a winner
            for (int i = 0; i < counter.length; i++) {
                if (counter[i] >= 100) {
                    status = true;
                    winner = runner[i];
                }
            }

            //delay code by half a second
            try {
                Thread.sleep(550);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //add spaces after the track has been executed
            for(int i =0; i<35; i++) {
                System.out.println();
            }

            //method to find the next position
            next_step(counter, competitor);

            // if to check that any animals are on the same spot
            if(counter[0] == counter[1]){
                for(int i =0; i <2; i++){
                    mini_counter[i] = counter[i];
                    mini_competitors[i] = competitor[i];
                }
                Same_Position(mini_counter, mini_competitors);
            }

            else if(counter[0] == counter[2]){
                mini_counter[0] = counter[0];
                mini_competitors[0] = competitor[0];
                mini_counter[1] = counter[2];
                mini_competitors[1] = competitor[2];

                Same_Position(mini_counter, mini_competitors);
            }

            else if(counter[1] == counter[2]){
                for(int i = 0; i < 2; i++){
                    mini_counter[i] = counter[(i+1)];
                    mini_competitors[i] = competitor[(i+1)];
                }
                Same_Position(mini_counter, mini_competitors);
            }

        } while (!status);
        return winner;
    }


    //method to determine how many steps the animal will move
    public static int[] next_step(int counter[], String competitor[]) {
        Random rand = new Random();

        for(int i = 0; i< counter.length; i++) {
            int rand_num = rand.nextInt(10) + 1;

            //nif to assign the number of space to move
            if (rand_num <= 5) {
                counter[i] += 3;
            }
            else if (rand_num <= 8) {
                counter[i] += 1;
            }
            //nested to ensure char isn't placed in position less than 1
            else {
                if (counter[i] <= 1) {
                    counter[i] = 1;
                }

                else if(counter[i] >= 5){
                    //announces to user that animal goes back
                    counter[i] -= 5;
                    System.out.println("OH NO! " + competitor[i] + " Slipped and fell Five Spaces!?");

                    //slow down execution speed so user can read
                    try {
                        Thread.sleep(1000); // Sleep to control the pace of the race (milliseconds)
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }
            }
        }
        return counter;
    }

    //method for when two animals land on the same position
    public static int[] Same_Position(int[] counter, String[] competitor){
        Random rand = new Random();
        int rand_num = rand.nextInt(8) + 1;

        //beeps to announce a collision
        Toolkit.getDefaultToolkit().beep();

            //possibility 1 delays and the other goes forward
            if(rand_num<5){
                int rand_num_2 = rand.nextInt(6) + 1;
                //one will move back while the other goes forward
                if(rand_num_2 < 4){
                    counter[0] -= 3;
                    counter[1] += 6;
                    System.out.println(competitor[0] + " & " + competitor[1]+ " Have Landed On The Same Spot!?");
                    System.out.println(competitor[0] + " has been kicked back! " + competitor[1] + " Uses the force to Go Forward!!!");
                } else{
                    counter[0] += 6;
                    counter[1] -= 3;
                    System.out.println(competitor[0] + " & " + competitor[1]+ " Landed On The Same Position!");
                    System.out.println(competitor[1] + " has been kicked back! " + competitor[0] + " Uses the force to Go Forward!!!");
                }

            }

            //possibility 2 both animals go back
            else{
                counter[0] -= rand.nextInt(6) + 1;
                counter[1] -= rand.nextInt(6) + 1;
                System.out.println(competitor[0] + " & " + competitor[1] + " Started to brawl!! They have both slipped down the slope!!");
            }

        //delay by 2 seconds
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        return counter;
    }
}
