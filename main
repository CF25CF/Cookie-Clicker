import pygame, json, tkinter as tk

def save():
    data = {
        "points": points,
        "upgrades": {
            "clicker": {"amount": up_clicker.amount, "price": up_clicker.price},
            "grandma": {"amount": up_grandma.amount, "price": up_grandma.price},
            "bakery": {"amount": up_bakery.amount, "price": up_bakery.price},
            "factory": {"amount": up_factory.amount, "price": up_factory.price},
        }
    }
    with open("save_data.json", "w") as file:
        json.dump(data, file)

def load():
    try:
        with open("save_data.json", "r") as file:
            return json.load(file)
    except FileNotFoundError:
        return {"points": 0,
                "upgrades": {
                "clicker": {"amount": 0, "price": 100},
                "grandma": {"amount": 0, "price": 500},
                "bakery": {"amount": 0, "price": 5000},
                "factory": {"amount": 0, "price": 30000}}
                }


auto_save_time1 = pygame.time.get_ticks()
def auto_save():
    global auto_save_time1
    time2 = pygame.time.get_ticks()
    if time2 - auto_save_time1 > 60000:
        save()
        auto_save_time1 = time2



loaded_data = load()

points = loaded_data["points"]
time1 = pygame.time.get_ticks()
shown_cps = 0
cps1 = 0
cps2 = 0
cookie1 = 0
Mouse_pressed = False



def cookie_points(window):
    points_font = pygame.font.Font("freesansbold.ttf", 70)
    points_text = points_font.render(str(points), True, (255, 255, 255))
    window.blit(points_text, (435, 35))
    window.blit(pygame.image.load("images/cookie_small.png"), (360, 38))

def show_cps_text(window):
    cps_font = pygame.font.Font("freesansbold.ttf", 40)
    cps_text = cps_font.render("+"+str(shown_cps), True, (255, 255, 255))
    window.blit(cps_text, (443, 120))

def cps_update():
    global time1, shown_cps, cookie1, cps1, cps2
    time2 = pygame.time.get_ticks()
    if time2 - time1 > 1000:
        time1 = time2
        cps3 = cps1
        shown_cps = cps3 - cps2
        cps2 = cps1


class Cookie:
    def __init__(self, x, y, image):
        self.x = x
        self.y = y
        self.image = image

    def draw(self, window):
        window.blit(self.image, (self.x, self.y))

    def clicked(self):
        global points, cps1
        click_sound = pygame.mixer.Sound("sounds/click_sound.wav")
        click_sound.set_volume(0.3)
        mouse_pos = pygame.mouse.get_pos()
        cookie_rect = pygame.Rect(self.x, self.y, self.image.get_width(), self.image.get_height())
        if cookie_rect.collidepoint(mouse_pos):
            click_sound.play()
            points += 1
            cps1 += 1
            return True



normal_cookie = Cookie(383, 250, pygame.image.load("images/cookie_normal.png"))
clicked_cookie = Cookie(395, 260, pygame.image.load("images/cookie_clicked.png"))
shown_cookie = normal_cookie


#upgrades are scaled to 80x80 pixels
class Upgrades:
    def __init__(self, name, x, y, price, cookie_gain, amount, time, image, dark_image=None):
        self.name = name
        self.x = x
        self.y = y
        self.price = price
        self.cookie_gain = cookie_gain
        self.amount = amount
        self.time = time
        self.image = image
        self.dark_image = dark_image


    def print_upgrade(self, window):
        global Mouse_pressed
        mouse_pos = pygame.mouse.get_pos()
        upgrade_rect = pygame.Rect(self.x, self.y, self.image.get_width(), self.image.get_height())

        if points >= self.price:
            if not Mouse_pressed:
                window.blit(self.image, (self.x, self.y))
            else:
                if upgrade_rect.collidepoint(mouse_pos):
                    window.blit(self.dark_image, (self.x, self.y))
                else:
                    window.blit(self.image, (self.x, self.y))
        else:
            window.blit(self.dark_image, (self.x, self.y))

        cost_font = pygame.font.Font("freesansbold.ttf", 25)
        upgrade_price_text = cost_font.render(str(self.price), True, (255, 255, 255))
        window.blit(upgrade_price_text, (self.x +20, self.y + 82))

        amount_font = pygame.font.Font("freesansbold.ttf", 25)
        amount_text = amount_font.render(str(self.amount), True, (255, 255, 255))
        window.blit(amount_text, (self.x - 15, self.y +5))



    def buy_upgrade(self):
        global points
        upgrade_sound = pygame.mixer.Sound("sounds/upgrade_sound.mp3")
        mouse_pos = pygame.mouse.get_pos()
        upgrade_rect = pygame.Rect(self.x, self.y, self.image.get_width(), self.image.get_height())
        if upgrade_rect.collidepoint(mouse_pos):
            if points >= self.price:
                upgrade_sound.play()
                upgrade_sound.set_volume(0.3)
                points -= self.price
                self.amount += 1
                self.price = round(self.price * 1.05)
                return True



    def gain_upgrades_clicker(self):
        global points, cps1
        time2 = pygame.time.get_ticks()
        if time2 - self.time > 1000:
            points += self.cookie_gain * self.amount
            cps1 += self.cookie_gain * self.amount
            self.time = time2

    def gain_upgrades(self):
        global points, cps1
        time2 = pygame.time.get_ticks()
        if time2 - self.time > 1000:
            points += self.cookie_gain * self.amount
            cps1 += self.cookie_gain * self.amount
            self.time = time2



up_clicker = Upgrades("clicker", 30, 50, 100, 1, 0, pygame.time.get_ticks(), pygame.image.load("images/up_cursor.png"), pygame.image.load("images/cursor_darker.png"))
up_grandma = Upgrades("grandma", 30, 175, 500, 6, 0,pygame.time.get_ticks(), pygame.image.load("images/grandma.png"), pygame.image.load("images/grandma_dark.png"))
up_bakery = Upgrades("bakery", 30, 300, 5000, 75, 0, pygame.time.get_ticks(), pygame.image.load("images/Bakery.png"), pygame.image.load("images/Bakery_dark.png"))
up_factory = Upgrades("factory", 30, 425, 30000, 500, 0, pygame.time.get_ticks(), pygame.image.load("images/Factory.png"), pygame.image.load("images/Factory_dark.png"))

up_clicker.amount = loaded_data["upgrades"].get("clicker", {}).get("amount", up_clicker.price)
up_grandma.amount = loaded_data["upgrades"].get("grandma", {}).get("amount", up_grandma.price)
up_bakery.amount = loaded_data["upgrades"].get("bakery", {}).get("amount", up_bakery.price)
up_factory.amount = loaded_data["upgrades"].get("factory", {}).get("amount", up_factory.price)


up_clicker.price = loaded_data["upgrades"].get("clicker", {}).get("price", up_clicker.price)
up_grandma.price = loaded_data["upgrades"].get("grandma", {}).get("price", up_grandma.price)
up_bakery.price = loaded_data["upgrades"].get("bakery", {}).get("price", up_bakery.price)
up_factory.price = loaded_data["upgrades"].get("factory", {}).get("price", up_factory.price)

settings_image = None

def draw_settings(window):
    global settings_image
    settings_image = pygame.image.load("images/Settings_image.png")
    window.blit(settings_image, (830, 0))


def open_settings():
    global settings_image
    if Mouse_pressed:
        mouse_pos = pygame.mouse.get_pos()
        rect = pygame.Rect(830, 0, settings_image.get_width(), settings_image.get_height())
        if rect.collidepoint(mouse_pos):

            root = tk.Tk()
            root.title("settings")
            root.geometry("300x200+800+300")
            root.maxsize(width=300, height=200)
            root.minsize(width=300, height=200)
            root.configure(bg="gray")




            def delete_save():
                global points, loaded_data
                import os
                if os.path.exists("save_data.json"):
                    os.remove("save_data.json")
                    loaded_data = load()
                    points = loaded_data["points"]
                    up_clicker.amount = loaded_data["upgrades"].get("clicker", {}).get("amount", up_clicker.price)
                    up_grandma.amount = loaded_data["upgrades"].get("grandma", {}).get("amount", up_grandma.price)
                    up_bakery.amount = loaded_data["upgrades"].get("bakery", {}).get("amount", up_bakery.price)
                    up_factory.amount = loaded_data["upgrades"].get("factory", {}).get("amount", up_factory.price)

                    up_clicker.price = loaded_data["upgrades"].get("clicker", {}).get("price", up_clicker.price)
                    up_grandma.price = loaded_data["upgrades"].get("grandma", {}).get("price", up_grandma.price)
                    up_bakery.price = loaded_data["upgrades"].get("bakery", {}).get("price", up_bakery.price)
                    up_factory.price = loaded_data["upgrades"].get("factory", {}).get("price", up_factory.price)

                    save()
                root.destroy()

            def save_button():
                save()

                root.destroy()

            save_button = tk.Button(root, text="save", command=save_button)
            save_button.pack(pady=10)

            delete_button = tk.Button(root, text="delete save", command=delete_save)
            delete_button.pack(pady=10)

            root.mainloop()



# RUN ###########################################################################



class Game:
    def __init__(self):
        pygame.init()
        pygame.display.set_caption("Cookie Clicker")
        self.window = pygame.display.set_mode((900, 560))
        self.background = pygame.image.load("images/background.png")
        self.clock = pygame.time.Clock()
        self.running = True

        pygame.mixer.init()
        pygame.mixer.music.load("sounds/Jorge Hernandez - Chopsticks ♫ NO COPYRIGHT 8-bit Music.mp3")
        pygame.mixer.music.play(-1)
        pygame.mixer.music.set_volume(0.7)



    def run(self):
        while self.running:
            global shown_cookie, points, Mouse_pressed

            self.window.fill("lightblue")
            self.window.blit(self.background, (0,0))
            self.clock.tick(60)

            cookie_points(self.window)
            show_cps_text(self.window)
            cps_update()
            shown_cookie.draw(self.window)
            draw_settings(self.window)


            up_clicker.print_upgrade(self.window)
            up_clicker.gain_upgrades_clicker()
            up_grandma.print_upgrade(self.window)
            up_grandma.gain_upgrades()
            up_bakery.print_upgrade(self.window)
            up_bakery.gain_upgrades()
            up_factory.print_upgrade(self.window)
            up_factory.gain_upgrades()

            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    save()
                    self.running = False
                if event.type == pygame.MOUSEBUTTONDOWN and event.button == 1:
                    Mouse_pressed = True
                    open_settings()
                    up_clicker.buy_upgrade()
                    up_grandma.buy_upgrade()
                    up_bakery.buy_upgrade()
                    up_factory.buy_upgrade()
                    if shown_cookie.clicked():
                        shown_cookie = clicked_cookie
                if event.type == pygame.MOUSEBUTTONUP:
                    Mouse_pressed = False
                    shown_cookie = normal_cookie

            auto_save()


            pygame.display.flip()
        pygame.quit()


if __name__ == "__main__":
    game = Game()
    game.run()
# RUN ###########################################################################


