#include <iostream>

#include <SFML/Graphics.hpp>
#include <SFML/Audio.hpp>
#include <SFML/System.hpp>
#include <SFML/Window.hpp>
#include <SFML/Network.hpp>
using namespace std;
using namespace sf;

int main()
{
    int score = 0;
    bool canjump = true;
    Clock animation_clock;
    Clock coin_clock;
    Clock goomba_clock;
    RenderWindow window(VideoMode(1700, 400), "SuperMario");

    RectangleShape layer(Vector2f(1700, 400.0));
    layer.setPosition(-100, 0);
    Texture sky;
    sky.loadFromFile("skyyy.png");
    layer.setTexture(&sky);

    RectangleShape layer2(Vector2f(1700.0, 390.0));
    layer2.setPosition(-100, 0);
    Texture sky2;
    sky2.loadFromFile("skyy.png");
    layer2.setTexture(&sky2);

    RectangleShape shape(Vector2f(75.0f, 75.0f));
    shape.setPosition(0, 300);
    Texture mario;
    mario.loadFromFile("mario4.png");
    shape.setTexture(&mario);
    shape.setScale(0.7, 1);
    shape.setTextureRect(IntRect(0, 0, 16, 32));
    int animationIndicator = 0;




    Texture goombas;
    goombas.loadFromFile("goomba.png");
    Sprite goomba(goombas);
    goomba.setPosition(700, 340);
    goomba.setTextureRect(sf::IntRect(0, 0, 36, 32));
    int  ganimationIndicator = 0;
    bool isgoombavisable = true;

    View camera;// (FloatRect(1700, 0, 1700, 400));
    camera.setCenter(Vector2f(350.0f, 200.0f));
    camera.setSize(Vector2f(800.0f, 400.0f));

    RectangleShape ground(Vector2f(75.0f, 75.0f));
    ground.setPosition(-100, 375);
    Texture Ground;
    Ground.loadFromFile("ground.png");
    ground.setTexture(&Ground);
    ground.setScale(13.4, 0.7);
    //ground.setTextureRect(IntRect(0, 0, 16, 32));
    //ground.setPosition(ground.getGlobalBounds().width, 400 - ground.getGlobalBounds().height);
    RectangleShape ground2(Vector2f(75.0f, 75.0f));
    ground2.setPosition(1000, 375);
    Texture Ground2;
    Ground2.loadFromFile("ground.png");
    ground2.setTexture(&Ground2);
    ground2.setScale(10, 0.7);

    RectangleShape ground3(Vector2f(75.0f, 75.0f));
    ground3.setPosition(300, 324);
    //ground3.setRotation(90);
    Texture Ground3;
    Ground3.loadFromFile("ground.png");
    ground3.setTexture(&Ground3);
    ground3.setScale(1, 0.7);

    Font font;
    font.loadFromFile("./Spider Home.ttf");
    Text text;
    text.setFont(font);
    text.setString("S c o r e = " + to_string(score));
    text.setFillColor(Color(0, 0, 0));
    text.setPosition(10, 10);
    text.setCharacterSize(35);
    text.setStyle(Text::Underlined);

    SoundBuffer buffer;
    buffer.loadFromFile("overworld.ogg");
    Sound sound;
    sound.setBuffer(buffer);
    sound.play();
    //bool sound getLoop();
    SoundBuffer bufferj;
    bufferj.loadFromFile("Mario 1 - Jump.ogg");
    Sound soundj;
    soundj.setBuffer(bufferj);




    Texture coinTexture;
    coinTexture.loadFromFile("coins.png");
    Sprite coin(coinTexture);
    coin.setPosition(500, 340);
    coin.setTextureRect(sf::IntRect(0, 0, 36, 32));
    int coin_animation_indicator = 0;
    bool isCoinVisable = true;


    SoundBuffer bufferc;
    bufferc.loadFromFile("coin.ogg");
    Sound soundc;
    soundc.setBuffer(bufferc);




    while (window.isOpen())
    {
       
        Event event;
        while (window.pollEvent(event))
        {
           
            if (event.type == Event::Closed) {

                cout << "End";
                window.close();

            }
            Time time;
            float start = 0;
        }

        if (event.type == Event::KeyPressed); {
        }
        if (Keyboard::isKeyPressed(Keyboard::D)) {
            camera.move(Vector2f(0.2, 0.0));
            text.move(Vector2f(0.2, 0.0));
            shape.move(Vector2f(0.2, 0.0));
            if (animation_clock.getElapsedTime().asSeconds() > 0.1) {
                animationIndicator++;
                animation_clock.restart();
            }
            shape.setScale(0.7, 1);

        }
        if (Keyboard::isKeyPressed(Keyboard::A)) {
            shape.move(Vector2f(-0.2, 0.0));
            camera.move(Vector2f(-0.2, 0.0));
            text.move(Vector2f(-0.2, 0.0));
            if (animation_clock.getElapsedTime().asSeconds() > 0.1) {
                animationIndicator++;
                animation_clock.restart();
            }
            shape.setScale(-0.7, 1);
        }
        animationIndicator = animationIndicator % 3;
        shape.setTextureRect(sf::IntRect(animationIndicator * 16, 0, 16, 32));

        if (shape.getGlobalBounds().intersects(coin.getGlobalBounds())) {
            isCoinVisable = false;
        }
        animationIndicator = animationIndicator % 3;
        shape.setTextureRect(IntRect(animationIndicator * 16, 0, 16, 32));
        if (shape.getGlobalBounds().intersects(coin.getGlobalBounds())) {
            coin.setScale(0, 0);
            score++;
            soundc.play();
            text.setString("score=" + to_string(score));
        }

        if (Keyboard::isKeyPressed(Keyboard::Key::Space) && canjump) {
            shape.move(Vector2f(0.0, -100.0));
            soundj.play();
            canjump = false;

        }
        if (!shape.getGlobalBounds().intersects(ground.getGlobalBounds())) {
            shape.move(Vector2f(0, 0.00001));

            if (!shape.getGlobalBounds().intersects(ground2.getGlobalBounds())) {
                shape.move(Vector2f(0, 0.00001));

                if (!shape.getGlobalBounds().intersects(ground3.getGlobalBounds())) {
                    shape.move(Vector2f(-0.00001, 0.00001));

                }
            }
        }
        else {
            canjump = true;
        }
        if (coin_clock.getElapsedTime().asSeconds() > 0.2) {
            coin_animation_indicator++;
            coin_clock.restart();

            coin.setTextureRect(sf::IntRect(coin_animation_indicator * 36, 0, 36, 32));
            coin_animation_indicator++;
            coin_animation_indicator = coin_animation_indicator % 6;
        }

        if (goomba_clock.getElapsedTime().asSeconds() > 0.2) {
            ganimationIndicator++;
            goomba_clock.restart();
            goomba.setScale(-1, 1);
            goomba.setTextureRect(sf::IntRect(ganimationIndicator * 16, 0, 16, 32));
            ganimationIndicator++;
            ganimationIndicator = ganimationIndicator % 2;
        }
        for (int i = 0; i < 100; i++)
        {
            if (!shape.getGlobalBounds().intersects(ground.getGlobalBounds()) && !shape.getGlobalBounds().intersects(ground2.getGlobalBounds()) && !shape.getGlobalBounds().intersects(ground3.getGlobalBounds()))
            {
                shape.move(0.0001, 0.001);
                shape.move(-0.0001, 0.001);
            }


        }
        window.setView(camera);
        window.clear();
        window.draw(layer);
        window.draw(layer2);
        window.draw(goomba);
        window.draw(text);
        window.draw(ground);
        window.draw(ground2);
        window.draw(ground3);
        if (isCoinVisable) window.draw(coin);

        window.draw(shape);
        window.display();
       

    }
    return 0;
}


