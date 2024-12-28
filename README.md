import requests

def finish(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        print('code finish work')
    return wrapper

@finish
def main():
    file_name = input('Enter file name:\t')
    url = input('URL be scan:\t')
    extension = input('Addresses end in("/" or ""):\t')
    formats = input('File format(separated by commas):\t')
    formats = [fmt.strip() for fmt in formats.split(",")]
    print(f"Formats to check: {formats}")
    print('Start scan...')



    f = open(file_name, 'rt')
    links = [line.strip() for line in f if any(line.strip().endswith(fmt) for fmt in formats)]

    f.close()

    if len(links) == 0:
        print('No links found')
        return

    result_scan = open('file.txt', 'w')

    for link in links:
        link = link.replace('\n', '')
        full_link = ''.join((url, link, extension))

        response = requests.get(
            full_link)

        if response.status_code != 404:
            if any(full_link.endswith(fmt) for fmt in formats):
                print(f'{full_link} - found')
                result_scan.write(full_link + '\n')
    result_scan.close()


if __name__ == "__main__":
    main()
