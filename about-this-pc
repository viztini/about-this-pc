import gi
gi.require_version("Gtk", "4.0")
from gi.repository import Gtk, Gdk
import platform
import psutil
import shutil

class AboutThisPC(Gtk.ApplicationWindow):
    def __init__(self, app):
        super().__init__(application=app)
        self.set_title("About This PC")
        self.set_default_size(600, 500)
        self.set_resizable(False)
        self.set_margin_top(25)
        self.set_margin_bottom(25)
        self.set_margin_start(25)
        self.set_margin_end(25)

        vbox = Gtk.Box(orientation=Gtk.Orientation.VERTICAL, spacing=20)
        self.set_child(vbox)

        icon = Gtk.Image.new_from_icon_name("computer")
        icon.set_pixel_size(128)  
        icon.set_halign(Gtk.Align.CENTER)
        vbox.append(icon)

        title = Gtk.Label()
        title.set_markup("<span size='22000'><b>About this PC</b></span>")
        title.set_halign(Gtk.Align.CENTER)
        vbox.append(title)

        card = Gtk.Box(orientation=Gtk.Orientation.VERTICAL, spacing=20)
        card.set_margin_top(25)
        card.set_margin_bottom(25)
        card.set_margin_start(25)
        card.set_margin_end(25)
        card.set_css_name("card")
        vbox.append(card)

        notebook = Gtk.Notebook()
        notebook.set_tab_pos(Gtk.PositionType.TOP)
        card.append(notebook)

        overview_box = Gtk.Box(orientation=Gtk.Orientation.VERTICAL, spacing=15)
        notebook.append_page(overview_box, Gtk.Label(label="Overview"))

        cpu_box = Gtk.Box(orientation=Gtk.Orientation.HORIZONTAL, spacing=10)
        cpu_icon = Gtk.Image.new_from_icon_name("computer-symbolic")
        cpu_label = Gtk.Label(label=f" CPU: {platform.machine()}")
        cpu_box.append(cpu_icon)
        cpu_box.append(cpu_label)
        overview_box.append(cpu_box)

        ram_box = Gtk.Box(orientation=Gtk.Orientation.HORIZONTAL, spacing=10)
        ram_icon = Gtk.Image.new_from_icon_name("input-keyboard-symbolic")
        ram_label = Gtk.Label(label=f" RAM: {round(psutil.virtual_memory().total / (1024**3))} GB")
        ram_box.append(ram_icon)
        ram_box.append(ram_label)
        overview_box.append(ram_box)

        os_box = Gtk.Box(orientation=Gtk.Orientation.HORIZONTAL, spacing=10)
        os_icon = Gtk.Image.new_from_icon_name("user-desktop-symbolic")
        os_label = Gtk.Label(label=f" OS: {platform.system()} {platform.release()}")
        os_box.append(os_icon)
        os_box.append(os_label)
        overview_box.append(os_box)

        kernel_box = Gtk.Box(orientation=Gtk.Orientation.HORIZONTAL, spacing=10)
        kernel_icon = Gtk.Image.new_from_icon_name("start-here-symbolic")
        kernel_label = Gtk.Label(label=f" Kernel: {platform.version()}")
        kernel_box.append(kernel_icon)
        kernel_box.append(kernel_label)
        overview_box.append(kernel_box)

        screen_box = Gtk.Box(orientation=Gtk.Orientation.HORIZONTAL, spacing=10)
        screen_icon = Gtk.Image.new_from_icon_name("video-display-symbolic")
        screen_label = Gtk.Label(label=f" Screen: {self.get_screen_resolution()}")
        screen_box.append(screen_icon)
        screen_box.append(screen_label)
        overview_box.append(screen_box)

        storage_box = Gtk.Box(orientation=Gtk.Orientation.VERTICAL, spacing=10)
        notebook.append_page(storage_box, Gtk.Label(label="Storage"))

        partitions = shutil.disk_usage("/")
        storage_label = Gtk.Label(
            label=f"Total: {round(partitions.total / (1024**3))} GB\n"
                  f"Used: {round(partitions.used / (1024**3))} GB\n"
                  f"Free: {round(partitions.free / (1024**3))} GB"
        )
        storage_box.append(storage_label)


        display_box = Gtk.Box(orientation=Gtk.Orientation.VERTICAL, spacing=10)
        notebook.append_page(display_box, Gtk.Label(label=" Displays"))

        display_label = Gtk.Label(label=f" Primary screen: {self.get_screen_resolution()}")
        display_box.append(display_label)

        css = b"""
        .card {
            border-radius: 25px;
            background-color: #f2f2f7;
            border: 1px solid #ccc;
            padding: 25px;
            box-shadow: 0 15px 30px rgba(0,0,0,0.2);
        }
        """
        style_provider = Gtk.CssProvider()
        style_provider.load_from_data(css)
        Gtk.StyleContext.add_provider_for_display(
            Gdk.Display.get_default(),
            style_provider,
            Gtk.STYLE_PROVIDER_PRIORITY_APPLICATION
        )

    def get_screen_resolution(self):
        display = Gdk.Display.get_default()
        monitors = display.get_monitors()
        if monitors:
            monitor = monitors[0]
            geo = monitor.get_geometry()
            return f"{geo.width} x {geo.height}"
        return " Unknown"

class AboutApp(Gtk.Application):
    def __init__(self):
        super().__init__(application_id="com.example.AboutThisPC")

    def do_activate(self):
        win = AboutThisPC(self)
        win.present()

if __name__ == "__main__":
    app = AboutApp()
    app.run()

